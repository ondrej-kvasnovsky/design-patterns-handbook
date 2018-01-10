# Comment to Better Name

It is very easy to name methods, files, classes or functions, if we don't care about other people, or about future us. This technique help to figure out better name for while we code.

The idea is to first start with a comment in the code. Then translate that comment to name.

### From a good comment to a good method name

Lets see an example. We want to create something what produces users and sends the produced users to a queue. We start with comment in the code. 

```
// produces users to a queue
```

Then we translate this comment to class name, for now. 

```
class UserQueueProducer {
}
```

Then we want to implement a method that will do the work. We write a command what that method is going to do. 

```
class UserQueueProducer {
    // produces a user and sends it to the queue
}
```

Then we translate the comment to a method name. 

```
class UserQueueProducer {
    // produces a user and sends it to the queue
    public void produceUserAndSendUserToQueue() {
    }
}
```

We always review the name, if it contains any duplications, we remove them. Or sometimes, we need to change the name to make it really understandable. 

```
class UserQueueProducer {
    public void produceAndSendUserToQueue() {
    }
}
```

### Good comment, bad name

Here is an example that shows only the first part done well, the comment provides enough information to justify what is going to happen. We have created very good comment for the method `Move all the remaining urls in one slot`. The the name does not reflect that good intention. 

```
StagingPlanRepository stagingPlanRepository = new StagingPlanRepository();

// Move all the remaining urls in one slot
stagingPlanRepository.updateSlotForAllUrls(1);
```

Here is how the method should be named. The consequence of making the method with proper name, can mean we need to create another method and fix the confusion. 

```
class StagingPlanRepository {
    public void moveAllRemainingUrlsInOneSlot() {
        updateSlotForAllUrls(1);
    }

    public void updateSlotForAllUrls(int slots) {
    }
}
```

Ideally, we would also rename `updateSlotForAllUrls` method. But that might be difficult, because we will need to go through the method code and figure out what the method is really doing.

