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

### Lambdas in java. Application 1

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
```
```java
sortList(Arrays.asList(1,3,5,2,9)); // works
sortList(Arrays.asList(3.14, 218.00, -1)); // does not :(. How to fix?

```
---

### Lambdas in java. Application 2

```java
public <T extends Comparable> void sortList(List<T> list) {
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
```
```java
sortList(Arrays.asList(1,3,5,2,9)); // works
sortList(Arrays.asList(3.14, 218.00, -1)); // works
sortList(Arrays.asList("abc", "abb", "aba", "aaa")); // works too
```

---

### Lambdas in java. Application 3

```java
@Data
public class SomeDTO { // implements nothing because ... reasons
    private String code;
    private boolean someFlag;
}
```
```java
sortList(Arrays.asList(
    new SomeDTO("01", true),
    new SomeDTO("02", false)
)); // compilation error, how to fix?
```

---

### Lambdas in java. Application 4

```java
public interface Comparator<T> {
    /* ...
     * @return a negative integer, zero, or a positive integer as the
     *         first argument is less than, equal to, or greater than the
     *         second.
     */
    int compare(T o1, T o2);
}
```

```java
public <T> void sortList(Comparator<T> comparator, List<T> list) {
   for (int i = 0; i < list.size(); i++) {
      for (int j = i+1; j < list.size(); j++) {
         if (comparator.compare(list.get(i),list.get(j)) > 0) {
            T tmp = list.get(i);
            list.set(i, list.get(j));
            list.set(j, tmp);
         }
      }
   }
   return list;
}
```

---

### Lambdas in java. Application 5

```java
sortList((o1, o2) -> o1.getCode().compareTo(o2.getCode()), Arrays.asList(
    new SomeDTO("01", true),
    new SomeDTO("02", false)
)); // works!
sortList((o1, o2) -> o1.getCreatedOn().compareTo(o2.getCreatedOn()), Arrays.asList(
    new SomeDTO("01", true),
    new SomeDTO("02", false)
)); // works too!
```
```java
sortList(
    (o1, o2) -> o1.getCreatedOn().equals(o2.getCreatedOn()) ?
        o1.getCreatedOn().compareTo(o2.getCreatedOn()) :
        o1.getCode().compareTo(o2.getCode()),
    Arrays.asList(
        new SomeDTO("01", true),
        new SomeDTO("02", false)
    )
); // and even this works
```

-----------------------------------------------------------------------------------

![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)

---

### Principles

- Declarative ( declarations as opposed to statements )
- Immutable data
- Functions are pure just like in math
