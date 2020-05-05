A Map is an unordered collection of Key Value Pairs in Groovy. The elements in a Map collection are accessed by it's key value.

## Example

```java
def map1 = [‘Name’ : ‘OneCompiler’, ‘Category’ : ‘Learning’]
def map2 = [:] // empty map
```
## Methods

| Method name | Description|
|----|----|
| containsKey() | Used to check if a key is present in a given Map|
| get() | Looks for the key in the map and returns it's corresponding value.If there is no match then returns null|
| keySet() | to obtain set of keys in the given Map.|
| put() | used to associate the given value with the key specified in the Map. If already value is present for that key, then the old value gets replaced by the new value.|
| size() | Returns the number of key-value pairs present in the Map.|
| values() | Returns a collection view of the values present in the Map.|

## Example

```java
def map1 = [Name:'OneCompiler', Category:'Learning']
println map1.get("Name")
println "Size of the map is "+ map1.size();
println "Values present in the map: " + map1.values();
println "Is Name present in the map? "+map1.containsKey("Name");
println "Is name present in the map? "+map1.containsKey("name");
```
### Check result [here](https://onecompiler.com/groovy/3vmvrbxmm)


