# Effective Java

## Item 1 : Consider static factory methods instead of constructors

```
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}

```

### Advantages :
- One advantage of static factory methods is that, unlike constructors, they
have names.
- A second advantage of static factory methods is that, unlike constructors,
they are not required to create a new object each time they’re invoked.
- A third advantage of static factory methods is that, unlike constructors,
they can return an object of any subtype of their return type.
- A fourth advantage of static factories is that the class of the returned
object can vary from call to call as a function of the input parameters.
- A fifth advantage of static factories is that the class of the returned object
need not exist when the class containing the method is written.

### Limitations :
- The main limitation of providing only static factory methods is that
classes without public or protected constructors cannot be subclassed.
    - For example, it is impossible to subclass any of the convenience implementation
      classes in the Collections Framework. Arguably this can be a blessing in disguise
      because it encourages programmers to use composition instead of inheritance
      (Item 18), and is required for immutable types (Item 17).
- A second shortcoming of static factory methods is that they are hard for
programmers to find.


#### Here are some common names for static factory methods. This list is far from exhaustive:
- <b>from</b> — A type-conversion method that takes a single parameter and returns a
corresponding instance of this type, for example:
`Date d = Date.from(instant);`
- <b>of</b> — An aggregation method that takes multiple parameters and returns an instance of this type that incorporates them, for example:
`Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);`
- <b>valueOf</b> — A more verbose alternative to from and of, for example:
`BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);`
- <b>instance or getInstance</b> — Returns an instance that is described by its parameters (if any) but cannot be said to have the same value, for example:
`StackWalker luke = StackWalker.getInstance(options);`
- <b>create or newInstance</b> — Like instance or getInstance, except that the
method guarantees that each call returns a new instance, for example:
`Object newArray = Array.newInstance(classObject, arrayLen);`
- <b>getType</b> — Like getInstance, but used if the factory method is in a different
class. Type is the type of object returned by the factory method, for example:
`FileStore fs = Files.getFileStore(path);`
- <b>newType</b> — Like newInstance, but used if the factory method is in a different
class. Type is the type of object returned by the factory method, for example:
`BufferedReader br = Files.newBufferedReader(path);`
- <b>type</b> — A concise alternative to getType and newType, for example:
`List<Complaint> litany = Collections.list(legacyLitany);`

---

## Item 2 : Consider a builder when faced with many constructor parameters

```
// JavaBeans Pattern - allows inconsistency, mandates mutability
public class NutritionFacts {
// Parameters initialized to default values (if any)
private int servingSize = -1; // Required; no default value
private int servings = -1; // Required; no default value
private int calories = 0;
private int fat = 0;
private int sodium = 0;
private int carbohydrate = 0;
public NutritionFacts() { }

// Setters
public void setServingSize(int val) { servingSize = val; }
public void setServings(int val) { servings = val; }
public void setCalories(int val) { calories = val; }
public void setFat(int val) { fat = val; }
public void setSodium(int val) { sodium = val; }
public void setCarbohydrate(int val) { carbohydrate = val; }
}

NutritionFacts cocaCola = new NutritionFacts();
cocaCola.setServingSize(240);
cocaCola.setServings(8);
cocaCola.setCalories(100);
cocaCola.setSodium(35);
cocaCola.setCarbohydrate(27);
```

Because construction is split across multiple calls, a JavaBean may be in an inconsistent state partway through its construction

A related disadvantage is that the JavaBeans pattern
precludes the possibility of making a class immutable (Item 17) and requires added effort on the part of the programmer to ensure thread safety.

```
// Builder Pattern
public class NutritionFacts {
private final int servingSize;
private final int servings;
private final int calories;
private final int fat;
private final int sodium;
private final int carbohydrate;
public static class Builder {
// Required parameters
private final int servingSize;
private final int servings;
// Optional parameters - initialized to default values
private int calories = 0;
private int fat = 0;
private int sodium = 0;
private int carbohydrate = 0;
public Builder(int servingSize, int servings) {
this.servingSize = servingSize;
this.servings = servings;
}
public Builder calories(int val)
{ calories = val; return this; }
public Builder fat(int val)
{ fat = val; return this; }
public Builder sodium(int val)
{ sodium = val; return this; }
public Builder carbohydrate(int val)
{ carbohydrate = val; return this; }
public NutritionFacts build() {
return new NutritionFacts(this);
}
}
private NutritionFacts(Builder builder) {
servingSize = builder.servingSize;
servings = builder.servings;
calories = builder.calories;
fat = builder.fat;
sodium = builder.sodium;
carbohydrate = builder.carbohydrate;
}
}
```

The NutritionFacts class is immutable, and hence it is thread safe also 

---


## Design Patterns mentioned in the book : 

- Flyweight
- Bridge
- Builder

