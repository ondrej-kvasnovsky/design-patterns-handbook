# Always Unit Test

There is a concept of [test pyramid](https://martinfowler.com/bliki/TestPyramid.html). It suggests that there should be more unit tests than integration tests. And there should be more integration tests than e2e tests. 

### When to write unit test

When we should create a unit test?

* If we follow Test Driven Development, we always start with a test. Most likely, it is going to be a unit test.
* If we don't follow TDD, we should create a unit test for each unit, a unit is a method or function.
* When we are fixing a bug \(and no tests are failing\), we should first replicate that by in a unit test. 

### Are there any exceptions

What are the exceptions? There can be a reason to not to write a unit test. The easy identifier of such a situation is length of setup for a unit test. If we have to mock half of the application, and our setUp method has a lot of lines, lets say more than 50, we might face to code which is not testable, it contains too many static initialization blocks and too many final keywords. We need to use tools like PowerMock, that can modify byte code and make the code testable by force.



