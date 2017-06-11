# Facade

Facade purpose is to hide complex libraries API and provide simple class or set of classes that are meant to be used by users of a library. 

### Example - Document reader

We are going to use Facade pattern to provide API for complex readers that can read various document types, like MS Word or Text files.

These are the readers that should be hidden from users.

```
class TextReader {

    public TextReader(String document) {
    }

    public String getText() {
        return "Content of a simple text fiel";
    }
}

class WordReader {

    public WordReader(String document) {
    }

    public String getText() {
        return "Content of a MS Word document.";
    }
}
```

We create a facade class called `DocumentReader` to provide easy API for library users. We usually put facade classes into library root, so it is the first thing a library user sees.

```
class DocumentReader {

    public String read(String document) {
        if (document.endsWith(".docx")) {
            return new WordReader(document).getText();
        } else if (document.endsWith(".txt")) {
            return new TextReader(document).getText();
        }

        return "";
    }
}
```

Here is how users of our library would use the facade. 

```
DocumentReader reader = new DocumentReader();

String wordText = reader.read("resume.docx");
String text = reader.read("resume.txt");
```



