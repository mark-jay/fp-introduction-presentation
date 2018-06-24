# Functional programming

Introduction

---

### Principles

- Declarative ( declarations as opposed to statements )
- Immutable data
- Functions are pure just like in math


---

### History - lambda calculus in 1930s


| Syntax        | Name           | Description  |
| ------------- |:--------------:| ------------:|
| x             | Variable       |	A character or string representing a parameter or value |
| (λx.M)        | Abstraction    |	Function definition (M is a lambda term). 'x' is bound in the expression. |
| (M N)	        | Application    |	Applying a function to an argument. M and N are lambda terms. |

---

### Lambda examples - turing complete

- λx.x+1
- (λx.x+1)3
- (λx.λy.(λz.(λx.z x) (λy.z y)) (x y))


### Lambdas in java

functions with no name and following syntax:
```java
 ()  -> 1
 ()  -> { System.out.println("Hello world"); }
 (x) -> { System.out.println("Hello " + x); return x; }
```

---

### Lisp programming language

- Originally specified in 1958, Lisp is the second-oldest high-level programming language in widespread use today
- Functional programming language based on the lambda calculus

---

![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)
