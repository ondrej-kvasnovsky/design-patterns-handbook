# Prototype

Prototype design pattern suggest to create a clone of a prototype rather than using `new` keyword and deep-copy all values to a clonned instance.

### Example - Simple Prototype

In order to implement prototype patern, we can simply implement Clonable interface from Java.

```
class HumanCell implements Cloneable {

    private String dna;

    public HumanCell(String dna) {
        this.dna = dna;
    }

    public String getDna() {
        return dna;
    }

    // all complex objects must be recreated (lists, maps, POJOs)
    public Object clone() throws CloneNotSupportedException {
        return new HumanCell(dna);
    }
}
```

Then we can clone human cells like this.

```
HumanCell cell = new HumanCell("DNA123");
HumanCell identical = (HumanCell) cell.clone();
```

### Example - Define own prototype interface

We can create our own interface that will define how a prototype should be copied into a new class.

```
interface Cell {
    Cell split();
    String getDna();
}

class SingleCellOrganism implements Cell {

    private String dna;

    public SingleCellOrganism(String dna) {
        this.dna = dna;
    }

    public String getDna() {
        return dna;
    }

    // all complex objects must be recreated (lists, maps, POJOs)
    public Cell split() {
        return new SingleCellOrganism(dna);
    }
}
```

Then we can use it as follows and we can exclude casting Object into a specific type.

```
Cell cell = new SingleCellOrganism("DNA123");
Cell identical = i1.split();
```



