[source: https://www.practical-go-lessons.com/post/how-to-read-a-file-line-by-line-with-go](https://www.practical-go-lessons.com/post/how-to-read-a-file-line-by-line-with-go-cbmaj47rttts70kq7c9g#:~:text=Scan

```go
package main

import (
   "bufio"
   "fmt"
   "log"
   "os"
)

func main() {
   myFile, err := os.Open("p/06_read_line_by_line/test.csv")
   if err != nil {
      log.Fatalf("impossible to open file: %s", err)
   }
   scanner := bufio.NewScanner(myFile)
   line := 0
   for scanner.Scan() {
      fmt.Printf("line: %d | Contents: %q \n", line, scanner.Text())
      line++
   }
   err = scanner.Err()
   if err != nil {
      log.Fatalf("scanner encountered an err: %s", err)
   }
}
```