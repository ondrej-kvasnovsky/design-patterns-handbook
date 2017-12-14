# Clarify Responsibility

What ever code we produce, we always clarify responsibility by refactoring. We might want to pick some of other design patterns to clarify the responsibility.

### Example - Builder

Now a days there are not many opportunities to find code that is really wrong and really dumb. I got lucky today because I got this beauty. This class pretends to be responsible for user creation. But is does also generates user ID using a datasource. There are also other things wrong: 

* stores and resets state in static variable
* is using Singleton pattern without reason
* all methods are static
* it is difficult to test
* is not thread safe, it this is called from multiple threads, it mixes up couple of users
* is using ancient naming conventions
* and there is no test for this class \(maybe luckily, there is not test\)

Here is the class, properly aged for 4 years in production. 

```
import org.apache.commons.lang.StringEscapeUtils;
import org.apache.commons.validator.routines.EmailValidator;
import org.mongodb.morphia.Datastore;

import java.util.HashSet;
import java.util.List;
import java.util.Set;

/**
 * Allows easy creation of valid, unique users in a given database.
 * <p>
 * This user will have a set ID based on the database is it intended for. The password will also be encrypted based on the PasswordService's encryptPassword function. All fields besides password and username will be Javascript escaped for Mongo. The username will be validated as an email address.
 *
 * @since June 2013
 */
public final class UserBuilder {

    /**
     * User builder instance to be returned.
     */
    private static final UserBuilder USER_INSTANCE = new UserBuilder();

    /**
     * The user to be CREATED.
     */
    private static UserDTO sUser = new UserDTO();

    /**
     * The plaintext version of the user's password.
     */
    private static String sPlaintextPassword = null;

    /**
     * The datastore which the user will be intended for.
     */
    private static Datastore sDatastore = null;

    /**
     * Private constructor to prevent instances being CREATED.
     */
    private UserBuilder() {
        // hide the constructor
    }

    /**
     * Reset the builder to its initial state. Mainly for testing.
     */
    protected static void reset() {
        sUser = new UserDTO();
        sDatastore = null;
        sPlaintextPassword = null;
    }

    /**
     * Returns the CREATED user after validating its fields for the datastore.
     * <p>
     * The user will have its password encrypted, and its ID will be set based on the provided datastore.
     *
     * @return The CREATED User object.
     */
    public static UserDTO create() throws RuntimeException {
        checkAllFieldsExist();
        if (sDatastore == null) {
            throwRuntimeException("Must provide datastore.");
        }
        sUser.setID(generateID());
        validatePassword();
        sUser.setCredentials(encryptPassword(sPlaintextPassword));
        validateUserEmail();

        final UserDTO toReturn = sUser;
        reset();
        return toReturn;
    }

    /**
     * Returns the CREATED user after validating its fields for the datastore.
     * <p>
     * The user will have its password encrypted, but no ID will be set and the datastore (if provided) will be ignored.
     *
     * @return The CREATED User object.
     */
    protected static UserDTO createWithoutDatastore() throws RuntimeException {
        checkAllFieldsExist();
        validatePassword();
        sUser.setCredentials(encryptPassword(sPlaintextPassword));
        validateUserEmail();

        final UserDTO toReturn = sUser;
        reset();
        return toReturn;
    }

    /**
     * The user will validated and its ID will be set for the given datastore.
     *
     * @param ds The database this user will be intended for after creation.
     * @return The UserBuilder instance.
     */
    public static UserBuilder forDatabase(final Datastore ds) {
        sDatastore = ds;
        return USER_INSTANCE;
    }

    /**
     * Set the username, which should be a valid email address.
     *
     * @param username The email and username for this user.
     * @return The UserBuilder instance.
     */
    public static UserBuilder withEmailAsUsername(final String username) {
        sUser.setUsername(username);
        sUser.setEmail(username);
        return USER_INSTANCE;
    }

    /**
     * Set the password, which will be encrypted.
     * <p>
     * The password should meet the following criteria: -8 characters or longer -Contains at least three of these character types: -lowercase letter -uppercase letter -special character -number
     *
     * @param password The user's password.
     * @return The UserBuilder instance.
     */
    public static UserBuilder withPassword(final String password) {
        sPlaintextPassword = password;
        return USER_INSTANCE;
    }

    /**
     * Set the username, which should be a valid email address.
     *
     * @param firstName The user's first name.
     * @param lastName  The user's last name.
     * @return The UserBuilder instance.
     */
    public static UserBuilder withName(final String firstName, final String lastName) {
        sUser.setFirstName(StringEscapeUtils.escapeJavaScript(firstName));
        sUser.setLastName(StringEscapeUtils.escapeJavaScript(lastName));
        return USER_INSTANCE;
    }

    /**
     * Set user's type.
     *
     * @param type The users type.
     * @return The UserBuilder instance.
     */
    public static UserBuilder withType(final String type) {
        sUser.setType(StringEscapeUtils.escapeJavaScript(type));
        return USER_INSTANCE;
    }

    /**
     * Set the user's roles. These will be used with Shiro's authentication.
     *
     * @param roles The roles for this user.
     * @return The UserBuilder instance.
     */
    public static UserBuilder withRoles(final String[] roles) {
        final Set<String> roleSet = new HashSet<>();
        for (final String r : roles) {
            roleSet.add(StringEscapeUtils.escapeJavaScript(r));
        }
        sUser.setRoles(roleSet);
        return USER_INSTANCE;
    }

    public static UserBuilder withAuthorizedGroups(final List<BatchGroup> authorizedGroups) {
        sUser.setAuthorizedGroups(authorizedGroups);
        return USER_INSTANCE;
    }

    private static void checkAllFieldsExist() throws RuntimeException {
        if (fieldInvalid(sUser.getUsername())) {
            throwRuntimeException("Must provide username.");
        }
        if (fieldInvalid(sPlaintextPassword)) {
            throwRuntimeException("Must provide password.");
        }
        if (fieldInvalid(sUser.getFirstName())) {
            throwRuntimeException("Must provide first name.");
        }
        if (fieldInvalid(sUser.getLastName())) {
            throwRuntimeException("Must provide last name.");
        }
        if (fieldInvalid(sUser.getType())) {
            throwRuntimeException("Must provide type.");
        }
        final Set roles = sUser.getRoles();
        if (roles == null || roles.size() == 0) {
            throwRuntimeException("Must provide roles.");
        }
    }

    private static boolean fieldInvalid(final String str) {
        return str == null || str.length() < 1;
    }

    private static void validateUserEmail() throws RuntimeException {
        final EmailValidator ev = EmailValidator.getInstance();
        if (!(ev.isValid(sUser.getEmail()) && ev.isValid(sUser.getUsername()))) {
            throwRuntimeException("Username was not a valid email address.");
        }
    }

    /*
     * Ensures that the password meets the following criteria: -8 characters or longer -Contains at least three of these character types: -lowercase letter -uppercase letter -special character -number
     */
    private static void validatePassword() throws RuntimeException {
        validatePassword(sPlaintextPassword);
    }

    public static void validatePassword(final String plainTextPassword) {

        if (plainTextPassword.length() < 8) {
            throwRuntimeException("Password is shorter than 8 characters.");
        }
        short hasNumber = 0;
        short hasLower = 0;
        short hasUpper = 0;
        short hasSpecial = 0;
        for (int i = 0; i < plainTextPassword.length(); ++i) {
            final char c = plainTextPassword.charAt(i);
            if ('a' <= c && c <= 'z') {
                hasLower = 1;
            } else if ('A' <= c && c <= 'Z') {
                hasUpper = 1;
            } else if ('0' <= c && c <= '9') {
                hasNumber = 1;
            } else {
                hasSpecial = 1;
            }
        }
        if (hasLower + hasNumber + hasUpper + hasSpecial >= 3) {
            return;
        }
        throwRuntimeException("Password must contain 3 of: lowercase, uppercase, numbers, special chars.");
    }

    public static void withDefaultProficiency(final String proficiency) {
        sUser.setProficiency(proficiency);
    }

    private static String encryptPassword(final String cleartext) {
        final PasswordService qps = new PasswordService();
        return qps.encryptPassword(cleartext);
    }

    private static String generateID() {
        final IdDAO IdDAO = new IdDAO(sDatastore);
        return IdDAO.generateId(User.class).toString();
    }

    private static void throwRuntimeException(final String message) throws RuntimeException {
        reset();
        throw new RuntimeException(message);
    }

    public static String getPassword(final String plainTextPassword) {
        validatePassword(plainTextPassword);
        return encryptPassword(plainTextPassword);
    }
}
```

Normally we would start with a test and then we could refactor the code in a responsible way. But this code is extremely hard to test. You would have to probably connect to database get MongoDB to test creation of users. Rather than that, we dare to modify it and as we refactor, we create tests to verify it is all working. 

We need to clarify responsibility of this class first. The most important thing to do is to remove dependency on `Database` and `PasswordService` classes. In order to do that, we will have to: 

* remove all static keywords from the code, we want to work with instance, not with a static variable
* extract code that generates password to other class, e.g. `IdGenerator`
* create methods `withEncryptedPassword(String password)` and `withId(String id)`
* replace `forDatabase` with `withId(String id)` method
* remove all class fields
* rename `create()` to `build()` because this class will be a builder that builds stuf
* remove `throwRuntimeException` method because it brings more code then actually executing
* fix misleading naming
* remove useless comments
* replace `fieldInvalid` method with a standard `isBlank` method
* move `validatePassword` to another class, e.g. `PasswordValidator`

Extract `generateID` into its own interface, because `IdGenerator` goes into database and we want to make our code testable. This way, we create a classes with clear responsibility, generate id. 

```
public interface IdGenerator {
    String generateID();
}
```

Implement `IdGenerator` for MongoDB.

```
import org.mongodb.morphia.Datastore;

@Component
public class IdGeneratorMongo implements IdGenerator {

    @Autowired
    @Qualifier("userDatastore")
    private Datastore datastore;

    public String generateID() {
        IdDAO idDAO = new IdDAO(datastore);
        return idDAO.generateId(User.class).toString();
    }
}
```

Create `PasswordValidator`, to creates a class with clear responsibility, and it is validation of password.

```
/**
 * Ensures that the password that is 8 characters or longer, contains at least three of these
 * character types: lowercase letter, uppercase letter, special character, number
 */
public class PasswordValidator {

    public void validatePassword(String plainTextPassword) {
        if (plainTextPassword.length() < 8) {
            throw new RuntimeException("Password is shorter than 8 characters.");
        }
        short hasNumber = 0;
        short hasLower = 0;
        short hasUpper = 0;
        short hasSpecial = 0;
        for (int i = 0; i < plainTextPassword.length(); ++i) {
            final char c = plainTextPassword.charAt(i);
            if ('a' <= c && c <= 'z') {
                hasLower = 1;
            } else if ('A' <= c && c <= 'Z') {
                hasUpper = 1;
            } else if ('0' <= c && c <= '9') {
                hasNumber = 1;
            } else {
                hasSpecial = 1;
            }
        }
        if (hasLower + hasNumber + hasUpper + hasSpecial >= 3) {
            return;
        }
        throw new RuntimeException("Password must contain 3 of: lowercase, uppercase, numbers, special chars.");
    }
}
```

Here is the refactored `UserBuilder`. `UserBuilder` has clarified responsibility: build a valid user object.

```
import org.apache.commons.lang.StringEscapeUtils;
import org.apache.commons.lang3.StringUtils;
import org.apache.commons.validator.routines.EmailValidator;

import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class UserBuilder {

    private UserDTO user;

    public UserBuilder() {
        this.user = new UserDTO();
    }

    public UserDTO build() throws RuntimeException {
        checkAllFieldsExist(user);
        validatePassword(user);
        validateUserEmail(user);
        return user;
    }

    public UserBuilder withId(String id) {
        user.setID(id);
        return this;
    }

    public UserBuilder withEmailAsUsername(final String email) {
        user.setUsername(email);
        user.setEmail(email);
        return this;
    }

    public UserBuilder withPassword(final String password) {
        user.setCredentials(password);
        return this;
    }

    public UserBuilder withName(final String firstName, final String lastName) {
        user.setFirstName(StringEscapeUtils.escapeJavaScript(firstName));
        user.setLastName(StringEscapeUtils.escapeJavaScript(lastName));
        return this;
    }

    public UserBuilder withType(String type) {
        user.setType(StringEscapeUtils.escapeJavaScript(type));
        return this;
    }

    public UserBuilder withRoles(String[] roles) {
        Set<String> roleSet = new HashSet<>();
        for (final String r : roles) {
            roleSet.add(StringEscapeUtils.escapeJavaScript(r));
        }
        user.setRoles(roleSet);
        return this;
    }

    public UserBuilder withAuthorizedGroups(List<BatchGroup> authorizedGroups) {
        user.setAuthorizedGroups(authorizedGroups);
        return this;
    }

    public UserBuilder withDefaultProficiency(String proficiency) {
        user.setProficiency(proficiency);
        return this;
    }

    private void checkAllFieldsExist(UserDTO user) throws RuntimeException {
        if (StringUtils.isBlank(user.getUsername())) {
            throw new RuntimeException("Must provide username.");
        }
        if (StringUtils.isBlank(user.getCredentials())) {
            throw new RuntimeException("Must provide password.");
        }
        if (StringUtils.isBlank(user.getFirstName())) {
            throw new RuntimeException("Must provide first name.");
        }
        if (StringUtils.isBlank(user.getLastName())) {
            throw new RuntimeException("Must provide last name.");
        }
        if (StringUtils.isBlank(user.getType())) {
            throw new RuntimeException("Must provide type.");
        }
        final Set roles = user.getRoles();
        if (roles == null || roles.size() == 0) {
            throw new RuntimeException("Must provide roles.");
        }
    }

    private void validateUserEmail(UserDTO user) throws RuntimeException {
        EmailValidator validator = EmailValidator.getInstance();
        if (!(validator.isValid(user.getEmail()) && validator.isValid(user.getUsername()))) {
            throw new RuntimeException("Username was not a valid email address.");
        }
    }

    private void validatePassword(UserDTO user) throws RuntimeException {
        new PasswordValidator().validatePassword(user.getCredentials());
    }
}
```

In order to see how the newly refactored will be used, have a look at this sample of test code that was created while refactoring.

```
import org.jetbrains.spek.api.Spek
import org.jetbrains.spek.api.dsl.given
import org.jetbrains.spek.api.dsl.it
import org.jetbrains.spek.api.dsl.on
import kotlin.test.assertEquals

class UserBuilderTest : Spek({
    given("builds a new user") {
        val id = "id"
        val username = "user@email.com"
        val password = "Password123!"
        val encryptedPassword = PasswordService().encryptPassword(password)
        val firstName = "firstName"
        val lastName = "lastName"
        val type = "type"
        val roles = arrayOf("role")
        val authorizedGroups = mutableListOf(BatchGroup())
        val proficiency = "proficiency"

        on("builds user") {
            val user = UserBuilder().withId(id)
                    .withEmailAsUsername(username)
                    .withPassword(encryptedPassword)
                    .withName(firstName, lastName)
                    .withType(type)
                    .withRoles(roles)
                    .withAuthorizedGroups(authorizedGroups)
                    .withDefaultProficiency(proficiency)
                    .build()

            it("sets all fields") {
                assertEquals(user.id, id)
                assertEquals(user.username, username)
                assertEquals(user.email, username)
                assertEquals(user.credentials, encryptedPassword)
                assertEquals(user.firstName, firstName)
                assertEquals(user.lastName, lastName)
                assertEquals(user.type, type)
                assertEquals(user.roles.stream().findFirst().get(), roles.get(0))
                assertEquals(user.authorizedGroups, authorizedGroups)
                assertEquals(user.proficiency, proficiency)
            }
        }
    }
})
```



