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
| x             |	Variable     |	A character or string representing a parameter or value |
| (λx.M)        |	Abstraction  |	Function definition (M is a lambda term). 'x' is bound in the expression. |
| (M N)	        | Application    |	Applying a function to an argument. M and N are lambda terms. |

---

### Lambda examples

- λx.x+1
- (λx.x+1)3 ( computations go as : (λx.x+1)3 ⇒ 3+1 ⇒ 4 )
- (λx.λy.(λz.(λx.z x) (λy.z y)) (x y))

---

![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)
