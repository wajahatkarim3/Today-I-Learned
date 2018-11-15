# Converting JSONObject to HashMap<>

Today, I had a huge Json in my app and I wanted to fetch all these values in a general way. I also wanted to parse Json for only one time to avoid `try/catch` each time I access any value. So, first thing I had in mind was to convert the `JSONObject` in the `HashMap` or `Map<>` etc.

So if you are using `Gson` in your apps for json mapping, then you can use this one line code for conversion.

```java
Map<String, Object> mapObj = new Gson().fromJson(
  myJsonObjectString, new TypeToken<HashMap<String, Object>>() {}.getType()
);
```
This will give you a `Map` object and you can access the values using this code.

```java
String strValue = (String) mapObj.get(myJsonKey);
String contains = mapObj.containsKey(myJsonKey);
```

## References
https://stackoverflow.com/questions/21720759/convert-a-json-string-to-a-hashmap
