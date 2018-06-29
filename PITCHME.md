# Functional programming

Introduction
https://github.com/mark-jay/fp-introduction-presentation/tree/master
https://gitpitch.com/mark-jay/fp-introduction-presentation/develop


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

### Closures in java

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

```
---

### Lambdas in java. Application 1.1.1

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
sortList(Arrays.asList(3.14, 218.00, -1)); // does not :(.
// How to fix?
```
---

### Lambdas in java. Application 1.2

```java
public <T extends Comparable> void sortList(List<T> list) {
    for (int i = 0; i < list.size(); i++) {
        for (int j = i+1; j < list.size(); j++) {
            if (list.get(i).compareTo(list.get(j)) > 0) {
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
public class SomeDTO { //implements nothing because...reasons
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
public interface Comparable<T> {
    /*
     * ...
     * @return  a negative integer, zero, or a positive integer as this object
     *          is less than, equal to, or greater than the specified object.
     * ...
     */
    public int compareTo(T o);
```

---

### Lambdas in java. Application 1.4.1

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
---

### Lambdas in java. Application 1.5.1

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
);
sortList(
    compareBy(x -> x.getCreatedOn()),
    getListToSort()
);
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

---

### Lambdas in java. Application 1.8

```java
sortList(
    comparing(SomeDTO::getCode),
    getListToSort()
);

sortList(
    comparing(SomeDTO::getCreatedOn),
    getListToSort()
);
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
    Arrays.asList(1, 2, 3, -1, -2, -3));
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
   Arrays.asList(1, 2, 3, -1, -2, -3));
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

---

### Lambdas in java. Application 2.4.1

more usages:

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

### Bash

find number of lines in the current project

```bash
$ find . -type f | grep java | xargs cat |wc -l
```

---

### Bash

find 5 biggest files in the project

```
find . -type f | grep 'java$' | xargs -n1 wc -l | sort -nr | head -n5
```

---

### RxJava

- pull based
- stream-like
- observer pattern
- composing asynchronous components
- supports different JVM langs

---

### RxAndroid

```java
retrofitService.getImage(url)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(bitmap -> myImageView.setImageBitmap(bitmap));
```

```java
ViewObservable.clicks(mCardNameEditText, false)
    .subscribe(view -> handleClick(view));
```

---

### Principles

- Declarative ( declarations as opposed to statements )
- Immutable data ( SOLID design, dependencies isolation, number of possible test cases )
- Functions are pure just like in math. ( And this can be proven by a compiler. Services analogy )

---

### Principles - Immutable data

- limited number of vars to remember
- less cases to test
- just like any other design pattern - SOLID.
-  ( SOLID design, dependencies isolation, number of possible test cases )
