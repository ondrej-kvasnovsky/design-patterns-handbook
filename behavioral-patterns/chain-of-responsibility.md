# Chain of Responsibility

Chain of responsibility pattern provides a way to create a linked list or pipeline of objects that might be able solve an issue the pipeline is asked to solve. 

### Example - Image processor

We want to create processor that will decide itself what algorithm should be executed for an given image type.

```
interface ImageTransformer {
    void setNext(ImageTransformer transformer);
    void resize(String image);
}

class JpegTransfomer implements ImageTransformer {

    private ImageTransformer next;

    @Override
    public void setNext(ImageTransformer transformer) {
        this.next = transformer;
    }

    @Override
    public void resize(String image) {
        if (image.endsWith(".jpeg")) {
            System.out.println(getClass().getSimpleName() + " is processing: " + image);
        } else {
            if (next != null) {
                next.resize(image);
            }
        }
    }
}

class PngTransfomer implements ImageTransformer {

    private ImageTransformer next;

    @Override
    public void setNext(ImageTransformer transformer) {
        this.next = transformer;
    }

    @Override
    public void resize(String image) {
        if (image.endsWith(".png")) {
            System.out.println(getClass().getSimpleName() + " is processing: " + image);
        } else {
            if (next != null) {
                next.resize(image);
            }
        }
    }
}
```

Then we can create the cain of image transformers and submit a request to process an image.

```
ImageTransformer chain = new PngTransfomer();
ImageTransformer jpegTransfomer = new JpegTransfomer();
chain.setNext(jpegTransfomer);

String png = "image.png";
String jpeg = "image.jpeg";

chain.resize(png);
chain.resize(jpeg);
```

Here is the output. 

```
PngTransfomer is processing: image.png
JpegTransfomer is processing: image.jpeg
```



