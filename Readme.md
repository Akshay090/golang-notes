# Learning Go

## Objects in Go
- Go does not use the term class.
- A struct is a collection of data.
- Go uses structs with associated methods.
- Go uses a simplified object orientation:
	- no inheritance
	- no constructors
	- no generics

## Concurrency
..is the management of multiple tasks that (could theoretically) run at the same time. Concurrent programming enables parallelism: 
- management of task execution
- communication between tasks
- synchronization between tasks 
Gorutines represent concurrent tasks
Channels are used to communicate between tasks
Select enables task synchronization 


## Pointer
- .. is an address to data in memory
- & operator returns  the address of some data
- * operator returns the data at some address
```go
var x int = 1			//declaration and initialisation of an int
var y int 			//declaration of an int
var ip *int			//declaration of a pointer to an int

ip = &x 				//setting ip to the address of x > ip now points to x
y = *ip				//setting y to the data stored at the address ip > y now is x > y = 1
----------------------------------------------------
var ptr *int = new(int) //new() creates a var and returns pointer to that var, therfore *int
*ptr = 3
fmt.Println(*ptr)		//3
fmt.Println(ptr)		//some address
```

## Variable Scope
A variable is accessible from block Bj if:
- the variable is declared in Bi and 
- Bi >= Bj

## Memory: Stack vs Heap
Stack: variables defined within a function 
	- Memory will be deallocated when function call completed (in C for example)
Heap: Global variables 
	- Needs to be explicitly deallocated ( in C for example), is fast but error-prone

## Type conversion
Use T() to convert, where T stands for the type you want to convert into
```go
var x int32 = 1
var x int16 = 2
x = y //error
x = int32(y)
```

## ASCII
Each char is associated with an 8-bit number. 
8 bits —> 0-255, not enough for different alphabets and symbols

## Unicode
Each char is associated with an 32-bit number. 
Unicode is variable in length. From 8-bits to 32-bits.
8-bit UTF codes are the same as ASCII

## Strings
— a sequence of bytes, each byte is a “rune” (Go specific, = UTF-8 code point)
Good packages: Unicode (for individual runes), String (for strings)
```go
var hello = "hello"
var test = hello[0]
fmt.Println(test) //104
fmt.Println(string(test)) //h
```

## Switch/case statement
If x is 1, enter case 1, if x is 2, enter case 2……..
Compared to C, no break statements are needed.  Go will not fall through, it automatically breaks.
```go
switch x {
case 1: 
	fmt.printf("case 1")
case 1: 
	fmt.printf("case 2")
default:
	fmt.printf("default case")
```

## Exported names
When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package. A name is exported if it begins with a capital letter.

## Variables
```go
var i, j int = 1, 2
var i, j = 1, 2 //int not needed, var will take the type of the initialiser

var c, python, java = true, false, "no!"
```

## Short variable declarations (only within functions)
```go
func main() {
	var i, j int = 1, 2
	k := 3    					//within fucntions we can omit the var keyword and use := 
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

## Functions

* Functions are first class - **allows functions to be assigned to variables, passed as arguments to other functions and returned from other functions.**

```go
func add(x, y int) int {
	return x + y
} //adds x and y, returns integer
```

### Function with multiple results

```go
func swap(x, y string) (string, string) {
	return y, x
} // parameter x and y get swapped, two strings get returned

```

### Named return values

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
} //a return statement without arguments, returns the named return values, here (x, y int)
```

### Variables as Functions

* Declare a variable as func

* ````go
  var funcVar func(int) int 
  func incFn(x int) int {
      return x + 1
  }
  func main() {
      funcVar = incFn // function on RHS without ()
      fmt.Print(funcVar(1))
  }
  ````

### Functions as Arguments

* Functions can be passes to another functions as arguments

```go
func applyIt(afunct func (int) int, 
             val int) int {
    return afunct(val)
}
func incFn(x int) int {
    return x + 1
} 
func main() {
    fmt.Print(applyIt(incFn, 2))
}
```

### Anonymous Functions

* Don't need to name a function

* ```go
  func applyIt(afunct func (int) int, 
               val int) int {
      return afunct(val)
  }
  
  func main() {
      v := applyIt(func (x int) int{return x + 1}, 2)
      fmt.Print(v)
  }
  ```

### Returning Functions and Closures

* Go supports anonymous functions, which can form closures

  ```go
  package main
  
  import "fmt"
  
  func intSeq() func() int {
      i := 0
      return func() int { // returns anonymous func
          i++             // it closes over variable i 
          return i		// to form closure
      }
  }
  
  func main() {
      nextInt := intSeq() // function value captures its own i value, which will be updated each time we call nextInt
  
      fmt.Println(nextInt())
      fmt.Println(nextInt())
      fmt.Println(nextInt())
  
      newInts := intSeq()
      fmt.Println(newInts())
  }
  ```

### Variadic and Deferred Functions 

* Variadic means function can take any number of arguments

  * **Variable Argument Number** 

    * Functions can take a variable no. of arguments

    * Uses ellipses **... **to specify

    * Treated as a slice inside function

    * ```go
      func getMax(vals, ...int) int {
          maxV := -1
          for _, v := range vals {
              if v > maxV {
                  maxV = V
              }
          }
          return maxV
      }
      ```

    * **Variadic Slice Argument** 

      * can pass slice to a variadic function

      * Need the **...** suffix 

      * ```go
        func main() {
            fmt.Println(getMax(1,3,6,4))
            vslice := []int{1,3,6,4}
            fmt.Println(getMax(vslice...))
        }
        ```

    * **Deferred Function Calls**

      * Call can be **deferred** until the surrounding function completes

      * Typically used for clean up activities

      * ````go
        func main() {
            i := 1
            defer fmt.Println(i+1)
            i++ 
            fmt.Println("Hello")
        }
        ````

## Arrays

- fixed length in Go
- Elements are accessed via [ ]
- Index starts at 0
- Elements initialized to zero value by default
```go
var x [5]int		 //declares array with 5 zeros of int type
x[0] = 2 			
________________________
//Array literals - An array pre-defined with values
var x [5] int = [5]{1,2,3,4,5}
x := [...]{1,2,3,4} 					//... is subsituted with  the length of the literal
___________________________
//Iterating through arry
for i, v range x {
	fmt.Printf("index %d, value %d", i, v) 
//range returns two values, the current index and the value at the index
```

## Slices
- A window on an underlying array, contain a pointer to array
- The **pointer** indicates the start of the array
- The **length** is the number of elements in the array
  - length of each slice is difference between it's declared indices
- The **capacity** is the max number of elements (from start of slice to end of array)
  - capacities are difference between **length of underlying array** and **starting index of slice**.

```go
array := [...]int{1,2,3,4,5}
slice1 := array[1:3] 				//2,3
slice2 := array[2:5] 				//3,4,5
fmt.Printf(cap(slice2)) 			// 3, because starting from index 2, the slice can have 3 elements 										at most
```

Length and Capacity

- **len()** function returns length 

* **cap()** function returns capacity

````go
a1 := [3]string{"a","b","c"}
sli1 := a1[0:1]
fmt.Printf(len(sli1),cap(sli1)) // 1 3
````

- changing a slice changes the underlying array
- overlapping arrays refer to the same elements, changing it in one slice, also changes it for the other element

## Slice literal
- creates the underlying array and then the slice that refers to it
- that means the slice and the array are the same.
- slice points to the start of the array, length equal to capacity
```go
//Sliche literal
sli := []{1,2,3} 			//slices don't need ... in the [ ]
```

## Variable slices
- you can also create a slice by using make()

- make either takes two to three arguments:

- Two: type and length 
 `sli = make([]int, 10)` // then length and capacity are equal

- Three arguments : : **type, length and capacity**
  `sli = make([]int, 10,15)` // underlying array len 15 and slice len 10

- **Append()**  lets you append elements. If there is space left in the underlying array, it will use that until it is full. Once full and you keep appending, it copies the content and creates a new, bigger underlying array

- ```go
   sli = make([]int, 0, 3) // len of sli is 0
   sli = append(sli, 100)
   fmt.Println(len(s), cap(s)) // 1 3
   ```


## Hash table
- contains key value pairs
- each value is associated with a unique key
- hash function is used to compute the slot for the key in the hash table. Input=key, output=slot to put value

### Maps (Go’s implementation of hash tables)

```go
var idMap map[string]int // key type string value type int
idMap = make(map[string]int)
_____________________________
//Map literal
idMap := map[string]int {
	"joe": 23}

_____________________________
//Accessing maps
fmt.Printf(idMap["joe"]) 				//prints 23

_____________________________
//Adding a key value pair
idMap["Jane"] = 34	//no key "Jane" so the key/val pair gets added

______________________________
//Editing a key value pair
idMap["Jane"] = 98					//overrides the old key/val pair

____________________________
//Deleting a key/val pair
delete(idMap, "joe"

_____________________________
//Two value assignment to test whether key is in map
id, b := idMap["joe"]					//id=23, b=true, because joe is a key in the map

_____________________________	
//Number of key/val pairs
len(idMap)

_____________________________
//Iterating through map
for key, val := range idMap {
	fmt.Prinf(key, val) 
}
```

## Struct
- Aggregate data type
- Groups together objects of arbitrary types
```go
type struct Person {
	name string
	addr string
	phone string
}
var p1 Person
p2 := new(Person) //creates new Person struct with all fields initialized to defaults 0 ir "" 
p1 := Person(name: "joe", addr: "a str", phone: "0123")			//struct literal
p1.name = "joe"
```

## Blank identifier
If an assignment requires multiple values on the left side, but one of the values will not be used by the program, a blank identifier on the left-hand-side of the assignment avoids the need to create a dummy variable and makes it clear that the value is to be discarded.
```go
if _, err := os.Stat(path); os.IsNotExist(err) {
	fmt.Printf("%s does not exist\n", path)
}
```

## JSON
- Format to represent structured Data
- Consists of Attribute-Value pairs
- All Unicode
- Types can be combined recursively
```go
//Go struct
type struct Person {
    name string
    addr string
    phone string
}

p1 := Person(name: "joh", age:24, phone:"039203")

//JSON
{"name": "joh", "age":23, "phone":"0320323"}

//Marshal creates JSON byte array from Go struct
barr, err := json.Marshal(p1)

//Unmarshal converts JSON []byte array into a Go struct
var p2 Person
err := json.Unmarshall(barr, &p2)

//the Go struct needs to have the same field names as the JSON object has attributes
//if Unmarshal worked, err is nil
```

## File Access
- Basic operations:
	- Open - get handle for access
	- Read - read bytes into []byte
	- Write - write []bytes into file
	- Close - release handle
	- Seek - move read/write head
- **Package “io/ioutil”**
```go
dat, err := ioutil.ReadFile("test.txt")
// dat is []byte filled with contents of entire file
//no open/close needed when using ReadFile
//don't use ReadFile with large files

dat = "Hello world"
err := ioutul.WriteFile("outfile.txt", dat, 0777)
// writes []byte to file, creates new file, unix style permission
```

## OS Package
- os.Open() -returns file descriptor
- os.Close()  - close a file
- os.Read() - reads from file into []byte, you can control how big []byte is
- os.Write() - writes []byte into file
```go
f, err := os.Open("test.txt)
barr := make([]byte, 10)
number, err := f.read(barr) 
f.close()
//fills barr with first 10 bytes from file, calling it the next time it will read the next 10 bytes --> head moves
//number = number of bytes read
```

## Methods 

* There are two types of methods : 
  1. Value receivers  : Methods that just do calculations on value, receive value is all they can do
  2. Pointer receivers : To change a value in struct we need a pointer receiver. You may want to use a pointer receiver type to avoid copying on method calls or to allow the method to mutate the receiving struct
     * No need to de-reference pointer with a *

* ```go
  package main
  
  import "fmt"
  
  type rect struct {
      width, height int
  }
  
  func (r rect) GetWidth() int {
      return r.width
  }
  
  func (r *rect) SetWidth(newWidth int) {
     r.width = newWidth // modifies struct
  }
  
  func main() {
      r := rect{width: 10, height: 5}
  
      fmt.Println("Width: ", r.GetWidth())
      r.SetWidth(110)	
      fmt.Println("Width: ", r.GetWidth())
  }
  ```

## Encapsulation

* A package is the smallest unit of private encap­sulation in Go.

* All identifiers defined within a [package](https://yourbasic.org/golang/packages-explained/) are visible throughout that package.

* When importing a package you can access only its **exported** identifiers.

* An identifier is exported if it begins with a **capital letter**.

  **Example**

  In this package, the only exported identifiers are `StopWatch` and `Start`.

  ```go
  package timer
  
  import "time"
  
  // A StopWatch is a simple clock utility.
  // Its zero value is an idle clock with 0 total time.
  type StopWatch struct {
      start   time.Time
      total   time.Duration
      running bool
  }
  
  // Start turns the clock on.
  func (s *StopWatch) Start() {
      if !s.running {
          s.start = time.Now()
          s.running = true
      }
  }
  ```

  The `StopWatch` and its exported methods can be imported and used in a different package.

  ```go
  package main
  
  import "timer"
  
  func main() {
      clock := new(timer.StopWatch)
      clock.Start()
      if clock.running { // ILLEGAL
          // …
      }
  }
  ```

  source: [yourbasic.org/golang/public-private](https://yourbasic.org/golang/public-private/)

## Interfaces

* Go doesn’t have inheritance – instead composition, embed­ding and inter­faces support code reuse and poly­morphism

* Interfaces is : Set of **method signatures**

  - name parameters, return value
  - Implementation is NOT defined

*  Used to express conceptual similarity between types

* **[interfaces](https://yourbasic.org/golang/interfaces-explained/)** take care of polymorphism and dynamic dispatch.

* reference : https://gobyexample.com/interfaces

* Type **satisfies an interface**  if type defines all methods specified in the interface.

  * same method signatures

* **Rectangle** and **Triangle** types satisfy the **Shape2D** interface

  * Must have Area() and Perimeter() methods
  * Additional methods are OK
  * Similar to inheritance and overriding

  ```go
  type Shape2D interface {
      Area() float64
      Perimeter() float64
  }
  type Triangle {...}
  func (t Triangle) Area() float64 {...}
  func (t Triangle) Perimeter() float64 {...}
  ```

  Triangle type satisfies the Shape2D interface, no need to state it explicitly.