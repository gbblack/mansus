Chapter 7: Maps
- Add element to a map: 
  `map_name[key] = {element}` -> `catalog[3] = Book{"The Sun Also Rises"}`
  Will overwrite existing element at that key if it already exists.
- Access an elemetn of a mpa:
  `map_name[key].field` -> `catalog[1].Title`
- To update en element in a map:
  First we must assign a new variable the new information and overwrite the old one, it cant be updated by accessing it directly.
  ```
  b := catalog[1]
  b.Title = "New Title"
  catalog[1] = b
  ```
- To check check whether a key exists in a map:
  `b, ok := catalog[4]`
  `ok` is a boolean value that tells you if the the element you are trying to access exists or not.