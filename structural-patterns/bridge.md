# Bridge

* Decouple an abstraction from its implementation so that the two can vary independently.
* Publish interface in an inheritance hierarchy, and bury implementation in its own inheritance hierarchy
* Beyond encapsulation, to insulation \(object wrapping\)

> At first sight, the Bridge pattern looks a lot like the Adapter pattern in that a class is used to convert one kind of interface to another. However, the intent of the Adapter pattern is to make one or more classes' interfaces look the same as that of a particular class. The Bridge pattern is designed to separate a class's interface from its implementation so you can vary or replace the implementation without changing the client code. [more](http://www.informit.com/articles/article.aspx?p=30297)

### Example - Document reader

We are going to create document parser that is responsible to read file and return its content as text. Implementation of `DocumentParser` will be different for MS Word, PDF and so on. Instead of using implementations of `DocumentParser` interface, we want to create `DocumentReader` because it will provide API for reading files.

```
interface DocumentParser {
    String parse();
}

class WordParser implements DocumentParser {

    private String fileName;

    public WordParser(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public String parse() {
        return "Content of Word document.";
    }
}

interface DocumentReader {
    String getText();
}

class DocumentReaderImpl implements DocumentReader {

    private DocumentParser documentParser;

    public DocumentReaderImpl(DocumentParser documentParser) {
        this.documentParser = documentParser;
    }

    @Override
    public String getText() {
        // TODO: here we would do more stuff if needed... 
        return documentParser.parse();
    }
}
```

Here is an example how to read MS Word using Bridge pattern.

```
DocumentReader reader = new DocumentReaderImpl(new WordParser("document.docx"));
String text = reader.getText();
```



