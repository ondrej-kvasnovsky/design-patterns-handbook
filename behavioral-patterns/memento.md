# Memento

Memento patter helps us to persist state of application in given time, so we can easily roll back or traverse through history of changes.

### Example - Save game progress

We are going to use memento pattern to save game whenever user reaches a checkpoint. 

```
class Game {
    private Checkpoint checkpoint;

    public void setCheckpoint(Checkpoint checkpoint) {
        this.checkpoint = checkpoint;
    }
}

class Checkpoint implements Cloneable {

    private int exp;

    public Checkpoint(int exp) {
        this.exp = exp;
    }

    public Checkpoint clone() {
        return new Checkpoint(exp);
    }
}

class GameProgressKeeper {

    private List<Checkpoint> checkpoints = new ArrayList<>();

    public void save(Checkpoint checkpoint) {
        checkpoints.add(checkpoint.clone());
    }

    public Checkpoint getLastOne() {
        return checkpoints.get(checkpoints.size() - 1);
    }
}
```

Here is how game could be played and what would have to happen in order to persist or roll back game state.

```
Game game = new Game();

// playing game... until the player reaches the first checkpoint
Checkpoint checkpoint1 = new Checkpoint(100); // other values...
game.setCheckpoint(checkpoint1);

GameProgressKeeper progressKeeper = new GameProgressKeeper();
progressKeeper.save(checkpoint1);

// continue playing game... until the player reaches the second checkpoint
// but the player died, roll back the game to the last saved checkpoint
progressKeeper.getLastOne();
game.setCheckpoint(checkpoint1);
```



