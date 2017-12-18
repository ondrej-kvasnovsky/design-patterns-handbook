# Keep Refactoring

There are the following false assumptions about code:

* it can be written well during when it is first implemented
* should be changed only if required by business
* not every class, file or function needs a test

We all know the code needs to be continuously integrated. We know the code needs to be continuously deployed. Also, we need to remember that the code needs to be contiously evolved and thus refactored.

In order to be able to refactor the code, we need to rely on tests. Tests are mandatory building block of software. 

Ignore everyone who is suggesting to put the refactoring changes into other pull requests, to keep changes separate and easier code review process. That there are too many changes in a pull request. They might ask you to put the refactoring changes into other pull requests, but it might be difficult to split the changes into multiple pull requests. The refactoring itself can be difficult. If we add one more difficulty, which is, how to split the changes to multiple pull request, we might end up not doing the refactoring at all. Because the refactoring become nearly impossible. Instead, suggest to and put the refactoring changes into separate commits.

