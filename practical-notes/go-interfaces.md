# Go Interfaces

- Small, focused interfaces that can be combined to create complex behavior
- Composition over inheritance
- Go simply requires you to define a type that has the same method signatures as the interface
- Interfaces is that they promote extensibility
- New kinds of structs can be added that implement the interface without changing the interface itself
- By defining interfaces, you can decouple them from the structs that implement them
- Easier to modify or replace the problematic structs without affecting other parts of the code
- Interfaces provide the abstractions you need, allowing you to write code that depends on an interface rather than a specific implementation (dependency inversion)
- Interface techniques:
  - Type assertion
  - Type switches
  - Interface embedding
  - Interface values
- Design interfaces that are small and focused, with a clear and well-defined purpose
- Have as few methods as possible for a given interface since implementing the interface requires defining the same methods for that particular struct with its own logic
- The three use cases that interfaces are useful for include “factoring out a common behavior, creating some decoupling, and restricting a type to a certain behavior”

---

- 


## Credits

- [Designing Extensible Software with Go Interfaces](https://earthly.dev/blog/software-design-go-interfaces/) by Ben Ben Smitthimedhin
- [Interface in Go](https://appmaster.io/blog/interface-implementation-go) by Robin Schmidt
