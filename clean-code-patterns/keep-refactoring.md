# Keep Refactoring

There are the following false assumptions about code:

* it can be written well during when it is first implemented
* should be changed only if required by business
* not every class, file or function needs a test

We all know the code needs to be continuously integrated, we need to merge our code often to avoid long living branches in our source code repository. We know the code needs to be continuously deployed, to get feedback if the code is deployable and working properly. Also, we need to remember that the code needs to be contiously evolved and thus refactored.

### How to make refactoring possible

In order to be able to refactor the code, we need to rely on tests. Tests are mandatory building blocks of software. If the application is not fully covered by tests, developers cannot consider a project as a success. Even when it was delivered on time. 

The code needs to be maintainable. In other words, other people who did not develop the code, must be willing to change it. They need to do it in a confident way. If they change the code, they run the tests. If tests fail, they know they did something wrong and that they need to go back and rethink the change.

> Sign of poor quality: "Applications or service is redeployed multiple times a day in order to fix a single bug."

### When we should refactor

This needs to be discussed within the team. But the general idea is to make the code better with each pull request. There could be  too many changes in a pull request. Therefore we should refactor only what is related to our changes.

> If we got spaghetti code, we might end up with huge refactoring pull requests, because everything is connected to everything. These refactorings should replace spaghetti code with better structured code. So other pull requests will be not that big.

Those who are suggesting to put the refactoring changes into a special "refactoring" pull requests, to keep changes separate and easier code review process, might be wrong. They might ask you to put the refactoring changes into other pull requests, but it might be difficult to split the changes into multiple pull requests. The refactoring itself can be difficult. If we add one more difficulty, which is, how to split the changes to multiple pull request, we might end up not doing the refactoring at all. Because the refactoring become nearly impossible. Instead, suggest to and put the refactoring changes into separate commits.

