# Structs

Since Go uses structs appart from classes the langiage doesn't fully support all
OOP principles such as classes, inheritance etc. It instead uses structs
compositions aspect.

```Go
package main

import "fmt"

// Base struct
type Person struct {
 Name string
 Age  int
}

// Composed struct
type Employee struct {
 Person     // embedded struct (composition)
 EmployeeID string
 Position   string
}

func main() {
 // Create an Employee
 emp := Employee{
  Person: Person{
   Name: "Alice",
   Age:  30,
  },
  EmployeeID: "E12345",
  Position:   "Developer",
 }

 // Accessing fields
 fmt.Println("Name:", emp.Name)         // Accessing embedded struct field directly
 fmt.Println("Age:", emp.Age)           // Accessing embedded struct field directly
 fmt.Println("Employee ID:", emp.EmployeeID)
 fmt.Println("Position:", emp.Position)

 // You can also access fields explicitly via the embedded struct
 fmt.Println("Person name (explicit):", emp.Person.Name)
}

```
