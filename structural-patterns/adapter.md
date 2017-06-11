# Adapter

Adapter lets classes work together that could not otherwise because of incompatible interfaces.

### Example

Lets say we work for eBay company and there are other users we need to integrate with our systems. Our, eBay, user is defined by `EbayUser` interface. External users are represented by class `ExternalUser`, but we do not want to mess with this class in our code and that is why we create an adapter that will adapt external user into our system.

```
interface EbayUser {
    String getUserName();
}

class EbayUserImpl implements EbayUser {

    private String userName;

    public EbayUserImpl(String userName) {
        this.userName = userName;
    }

    @Override
    public String getUserName() {
        return userName;
    }
}

class ExternalUserToEbayUserAdapter implements EbayUser {

    private String userName;

    public ExternalUserToEbayUserAdapter(ExternalUser payPalUser) {
        userName = payPalUser.getUsername();
    }

    @Override
    public String getUserName() {
        return userName;
    }
}

class ExternalUser {

    private String username;

    public ExternalUser(String username) {
        this.username = username;
    }

    public String getUsername() {
        return username;
    }
}
```

Here is an example how we could adapt an external user into our system.

```
ExternalUser externalUser = new ExternalUser("john");

EbayUser ebayUser = new ExternalUserToEbayUserAdapter(externalUser);
```



