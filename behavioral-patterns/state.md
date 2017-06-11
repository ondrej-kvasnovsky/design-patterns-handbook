# State

* Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.
* An object-oriented state machine
* wrapper + polymorphic wrappee + collaboration

### Example - UI states

We are going to use state pattern to implement different states of a login screen. 

```
class TextField {
}

class Button {
}

interface State {
    void usernameTextField(TextField textField);

    void passwordTextField(TextField textField);

    void loginButtonTextField(Button button);
}

class EmptyState implements State {

    @Override
    public void usernameTextField(TextField textField) {
        // textField.setEnabled(true);
    }

    @Override
    public void passwordTextField(TextField textField) {
        // textField.setEnabled(true);
    }

    @Override
    public void loginButtonTextField(Button button) {
        // textField.setEnabled(false);
    }
}

class ReadyState implements State {

    @Override
    public void usernameTextField(TextField textField) {
        // textField.setEnabled(true);
    }

    @Override
    public void passwordTextField(TextField textField) {
        // textField.setEnabled(true);
    }

    @Override
    public void loginButtonTextField(Button button) {
        // textField.setEnabled(true);
    }
}

class UserLoginUI {
    private State state;

    public UserLoginUI() {
        this.state = new EmptyState();
    }

    public void applyState(State state) {
        this.state = state;
        this.state.usernameTextField(null);
        this.state.passwordTextField(null);
        this.state.loginButtonTextField(null);
    }
}
```

Here is how we would use the states. 

```
UserLoginUI loginUI = new UserLoginUI();

// user just opened the login dialog
loginUI.applyState(new EmptyState());

// when user fills in username and password
loginUI.applyState(new ReadyState());

// when user clicks on Login button and it fails
//loginUI.setState(new ErrorState());
```



