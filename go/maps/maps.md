Maps is an unordered collection of key and its value. Map is one of the powerful data types provided by Go.  Key is used to retrieve it's associated value at a later stage and they are pretty fast in retrieving the value.

## How to declare a Map?

```c
var map-name map[key-data-type]value-data-type
```

## How to define a Map?

```c
map-name = make(map[key-data-type]value-data-type)
```

## Example for Insert & Update Operations in Maps

```c
package main
import "fmt"

func main() {
 
  var nationalGame map[string]string // declaring a map
 
  nationalGame = make(map[string]string) // defining a map
   
  /* Inserting key & it's value to map*/
  nationalGame["Australia"] = "Cricket"
  nationalGame["Japan"] = "Wrestling"
  nationalGame["NewZealand"] = "Rugby"
  nationalGame["USA"] = "Baseball"
  nationalGame["England"] = "Cricket"

  for nation := range nationalGame {
    fmt.Println(nation," : ",nationalGame[nation])
  }
  
  // Update operation in maps
  nationalGame["India"] = "Hockey"
  fmt.Println("\nResults are updating India are: ")
  for nation := range nationalGame {
    fmt.Println(nation," : ",nationalGame[nation])
  }
  
}
```
### Check result [here](https://onecompiler.com/go/3vpyyghys)

## Example for retrieve operation

```c
package main
import "fmt"

func main() {
 
  var nationalGame map[string]string // declaring a map
 
  nationalGame = make(map[string]string) // defining a map
   
  /* Inserting key & it's value to map*/
  nationalGame["Australia"] = "Cricket"
  nationalGame["Japan"] = "Wrestling"
  nationalGame["NewZealand"] = "Rugby"
  nationalGame["USA"] = "Baseball"
  nationalGame["England"] = "Cricket"

  /* Retrieving value using it's key */
  fmt.Println("National game of England is: ", nationalGame["England"])

}
```

### Check result [here](https://onecompiler.com/go/3vpyzxt96)

## Example for delete operation

delete() is used to delete a value using it's key from a map.

```c
package main
import "fmt"

func main() {
 
  var nationalGame map[string]string // declaring a map
 
  nationalGame = make(map[string]string) // defining a map
   
  /* Inserting key & it's value to map*/
  nationalGame["Australia"] = "Cricket"
  nationalGame["Japan"] = "Wrestling"
  nationalGame["NewZealand"] = "Rugby"
  nationalGame["USA"] = "Baseball"
  nationalGame["England"] = "Cricket"
  fmt.Println(nationalGame)

  /* Deleting value using it's key */
  delete(nationalGame, "Japan")
  fmt.Println("\nMap results after deleting Japan are:")  
  fmt.Println(nationalGame)
}
```

## How to check if a key is present in a map?
```c
package main
import "fmt"

func main() {
 
  var nationalGame map[string]string // declaring a map
 
  nationalGame = make(map[string]string) // defining a map
   
  /* Inserting key & it's value to map*/
  nationalGame["Australia"] = "Cricket"
  nationalGame["Japan"] = "Wrestling"
  nationalGame["NewZealand"] = "Rugby"
  nationalGame["USA"] = "Baseball"
  nationalGame["England"] = "Cricket"

/* Checking if England if present in the map */
   value, isPresent := nationalGame["England"]  
   fmt.Println("Is England is Present in the map? ", isPresent, "\nEngland's national game is: ", value)  
   
/* Checking if India if present in the map */
   value1, isPresent1 := nationalGame["India"]  
   fmt.Println("Is India is Present in the map? ", isPresent1, value1)  
}
```
### Check result [here](https://onecompiler.com/go/3vpz34prz)