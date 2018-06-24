# Functional programming

Introduction

---

### Overview

- History. Why lambdas and closures?


---

### History - lambda calculus, introduced in 1930s


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

---

### Lisp programming language

- Originally specified in 1958, Lisp is the second-oldest high-level programming language in widespread use today
- Functional programming language based on the lambda calculus
- "Lisp programmers know the value of everything but the cost of nothing"

---

### Lambdas syntax in java

In java lambdas are functions with no name and with the following syntax:

```java
 ()  -> 1
 ()  -> { System.out.println("Hello world"); }
 (x) -> { System.out.println("Hello " + x); return x; }
```

---

### Funarg problem and solution. Closures in java

```java
public Supplier<Integer> getRandomizer(int seed) {
    Random random = new Random(seed);
    return () -> random.nextInt();
}
```
---

### Lambdas in java. Application

```java
public void sortList(List<Integer> list) {
    for (int i = 0; i < list.size(); i++) {
        for (int j = i+1; j < list.size(); j++) {
            if (list.get(i) > list.get(j)) {
                Integer tmp = list.get(i);
                list.set(i, list.get(j));
                list.set(j, tmp);
            }
        }
    }
    return list;
}
...
sortList(Arrays.asList(1,3,5,2,9)); // works
sortList(Arrays.asList(3.14, 218.00, -1)); // does not :(. How to fix?

```
---

### Lambdas in java. Application

```java
public <T> void sortList(List<T> list) {
    for (int i = 0; i < list.size(); i++) {
        for (int j = i+1; j < list.size(); j++) {
            if (list.get(i) > list.get(j)) {
                T tmp = list.get(i);
                list.set(i, list.get(j));
                list.set(j, tmp);
            }
        }
    }
    return list;
}
...
sortList(Arrays.asList(1,3,5,2,9)); // works
sortList(Arrays.asList(3.14, 218.00, -1)); // works
sortList(Arrays.asList("abc", "abb", "aba", "aaa")); // works too

```

---

![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)

---

### Principles

- Declarative ( declarations as opposed to statements )
- Immutable data
- Functions are pure just like in math
