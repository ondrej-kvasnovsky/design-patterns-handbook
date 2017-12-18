# It Will Never Happen

Natural part of programmers' work is to try to predict what will or not happen. For example, if user does not provide password while authenticating, the program reports a failure. This is correct assumption and this logical flow should be implemented. 

Sometime, we go too far and we try to predict what will never happen. Look at this code.  
   ![](/assets/itwillneverhappen.png)  
  
The original author was sure that else branch should never be executed. But when I was debugging the application, I found it is actually called. But it shouldn't be called, how come it is called now? This application is deployed in production and this code is not causing issues. Everything works perfectly. This else branch is executed because the code around was changed and the other code around stopped carrying what `getCurrentUser` method returns. 

What we should do when we think this code will never happen? We could consider these options. 

* don't implement that "will or should never happen" code, if it should never happen, it shouldn't happen to be implemented
* throw an exception, indicating this should never happen, so users of the code will know something is very wrong
* log fatal error, so users of the code will know something is very wrong



