# Spaghetti Code {#firstHeading}

We have spaghetti code in the project when the flow is conceptually like a bowl of spaghetti, i.e. twisted and tangled. There are other [Italian type of code](https://en.wikipedia.org/wiki/Spaghetti_code) as well.

### Example

I have fount this interesting spaghetti code. The code is nicely tangled, very very hard to decode what is happening \(very hard to follow where the spaghetti goes\).

```
public boolean startElement(int, XMLAttrList) throws Exception {
  ...
  if (... && !fValidating && !fNamespacesEnabled) {
    return false; }
  ...
  if (contentSpecType == -1 && fValidating) {
  ... }
  if (... && elementIndex != -1) { ...
  }
  if (DEBUG_PRINT_ATTRIBUTES) {
    if (fNamespacesEnabled) { fNamespacesScope.increaseDepth(); if (attrIndex != -1) {
      int index = attrList.getFirstAttr(attrIndex); while (index != -1) {
    } index =
    } 
  }
  ...  
  if (fStringPool.equalNames(...)) {
    ...
  } else {...}
  attrList.getNextAttr(index);
  int prefix = fStringPool.getPrefixForQName(elementType); int elementURI;
  if (prefix == -1) {
  ...
  if (elementURI != -1) {
    fStringPool.setURIForQName(...); }
  } else { ...
  if (elementURI == -1) { ...
  }
  fStringPool.setURIForQName(..., elementURI); }
  if (attrIndex != -1) {
  int index = attrList.getFirstAttr(attrIndex); while (index != -1) {
  int attName = attrList.getAttrName(index); 
  if (!fStringPool.equalNames(...)) {
  ...
  if (attPrefix != fNamespacesPrefix) { if (attPrefix == -1) {
  ... }else{
  if (uri == -1) {
    ...
  }
  fStringPool.setURIForQName(attName, uri);
  ... }
  if (fElementDepth >= 0) { ...
  }
  fElementDepth++;
  if (fElementDepth == fElementTypeStack.length) {
  ... }
  ...
 return contentSpecType == fCHILDRENSymbol; 
 }
```



