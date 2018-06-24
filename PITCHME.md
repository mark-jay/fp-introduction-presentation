# Functional programming

Introduction

---

### Overview

- History. Why lambdas and closures?
- Lambdas in java. Sort example
- Lambdas in java. Streams
- Principles

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

### Lambdas in java. Application 1.1

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

### Lambdas in java. Application 1.2

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

### Lambdas in java. Application 1.3

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

### Lambdas in java. Application 1.4

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

### Lambdas in java. Application 1.5

```java
sortList(
    (o1, o2) -> o1.getCode().compareTo(o2.getCode()),
    getListToSort()
); // works!
sortList((o1, o2) -> o1.getCreatedOn().compareTo(o2.getCreatedOn()),
    getListToSort()
); // works too!
```
```java
sortList(
    (o1, o2) -> o1.getCreatedOn().equals(o2.getCreatedOn()) ?
        o1.getCode().compareTo(o2.getCode()) :
        o1.getCreatedOn().compareTo(o2.getCreatedOn()),
    getListToSort()
);
// and even this works.
// But how to fix boilerplate we introduced?
// And what if it's null?
```

---

### Lambdas in java. Application 1.6

```java
sortList(
    compareBy(x -> x.getCode()),
    getListToSort()
); // works!
sortList(
    compareBy(x -> x.getCreatedOn()),
    getListToSort()
); // works too!
```
```java
sortList(
    compareBy(x -> x.getCreatedOn(), x -> x.getCode()),
    getListToSort()
);
```

---

### Lambdas in java. Application 1.7

```java
    public static <T, U extends Comparable<U>>
        Comparator<T> comparing(
            Function<T, U> keyExtractor)
    {
        return (c1, c2) -> keyExtractor.apply(c1)
                .compareTo(keyExtractor.apply(c2));
    }

```
```java
    public static
        <T, U extends Comparable<? super U>> Comparator<T>
        comparing(Function<? super T, ? extends U> keyExtractor)
    {
        Objects.requireNonNull(keyExtractor);
        return (Comparator<T> & Serializable)
            (c1, c2) -> keyExtractor.apply(c1)
                 .compareTo(keyExtractor.apply(c2));
    }
```

---

### Lambdas in java. Application 1.8

```java
sortList(
    comparing(SomeDTO::getCode),
    getListToSort()
); // works!
sortList(
    comparing(SomeDTO::getCreatedOn),
    getListToSort()
); // works too!
```
```java
sortList(
    comparing(SomeDTO::getCreatedOn)
        .thenComparing(SomeDTO::getCode)
    getListToSort()
);
```

---

### Lambdas in java. Application 2.1

```java
List<Integer> list = new ArrayList<Integer>(
    Arrays.asList(1,2,3,-1,-2,-3));
```
```java
for (Integer i : list) {
    if (i < 0) {
        list.remove(str);
    }
}
for (Integer i : list) {
    System.out.println(i);
}
```


---

### Lambdas in java. Application 2.2

```java
List<Integer> list = new ArrayList<Integer>(
   Arrays.asList(1,2,3,-1,-2,-3));
```
```java
ArrayList<Integer> result = new ArrayList<>();
for (Integer i : list) {
    if (i >= 0) {
        result.add(i); // no concurrent modification exception
    }
}
for (Integer i : result) {
    System.out.println(i);
}
```

---

### Lambdas in java. Application 2.3

```java
List<Integer> list = new ArrayList<Integer>(Arrays.asList(1,2,3,-1,-2,-3));
```
```java
ArrayList<Integer> result = list.stream()
    .filter(i -> i >= 0)
    .collect(Collectors.toList());
result.foreach(System.out::println);
```

---

### Lambdas in java. Application 2.4

more usages:

```java
Double totalDebtAmount = userList.stream()
    .filter(user -> user.createdOn().after(someDate))
    .mapToDouble(User::getDebtAmount)
    .sum();
```

```java
"Email : " + user.getContact().getPrimaryContactEmail()
    .getEmail(); // ->
"Email : " + Optional.of(user)
    .map(User::getContact)
    .map(Contact::getPrimaryContactEmail)
    .map(ContactEmail::getEmail)
    .orElse("n/a");
```

---

### Principles

- Declarative ( declarations as opposed to statements )
- Immutable data ( SOLID design, dependencies isolation, number of possible test cases )
- Functions are pure just like in math. ( And this can be proven by a compiler. Services analogy )


---

-----------------------------------------------------------------------------------

![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)
