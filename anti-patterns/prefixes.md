# Prefixes

Somehow, package name is not enough to identify a class. We should think harder before we decide to prefix all classes names with, for example company name or company first letter.

### Company or product prefixes

Lets say we work for company named `UmpaLumpaOfScience` that is developing a product named `science`. We should probably avoid using these kind of prefixes. 

If there is a conflict in names in one class, probably something else is wrong our class design and we need to rethink how our classes exchange data and how they work together. 

```
public class UUser {
}

public class UScienceUser extends UUser {
}
```

### `m` for member `s` for static

Just don't do this.

```
public class Task {

  private static final String[] sExcludedFields = new String[]{"mCreated", "mId"};

  private Integer mId;

  private String mTemplate;

  public Integer getId() {
    return mId;
  }
  
  public setId(Integer id) {
    return mId = id;
  }
  
  public String getTemplate() {
    return mTemplate;
  }
  
  public getTemplate(String template) {
    mTemplate = template;
  }
  
  @Override
  public String toString() {
    return ToStringBuilder.reflectionToString(this, SHORT_PREFIX_STYLE);
  }

  @Override
  public int hashCode() {
    return HashCodeBuilder.reflectionHashCode(this, sExcludedFields);
  }

  @Override
  public boolean equals(final Object obj) {
    return EqualsBuilder.reflectionEquals(this, obj, EXCLUDE_FIELDS);
  }
}
```



