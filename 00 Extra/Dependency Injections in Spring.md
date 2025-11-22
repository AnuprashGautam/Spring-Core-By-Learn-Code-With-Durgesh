# ✅ Dependency Injection in Spring (Core Spring Framework)

In Spring Framework, you can inject dependencies using:

1. **Constructor Injection**
2. **Setter Injection**
3. **Field Injection** (annotation-based)

But the **main difference** from Spring Boot is:
👉 *In Spring, you configure dependencies either through **XML configuration** or **Annotations (@Component + @Autowired)**.*

Let’s understand both.

---

# 1️⃣ **Constructor Injection (Spring XML & Annotation Examples)**

### **XML-based Constructor Injection**

```xml
<bean id="engine" class="com.example.Engine" />

<bean id="car" class="com.example.Car">
    <constructor-arg ref="engine" />
</bean>
```

### **Car.java**

```java
public class Car {
    private Engine engine;

    public Car(Engine engine) {    // constructor injection
        this.engine = engine;
    }

    public void drive() {
        engine.start();
    }
}
```

### When to Use?

✔ Best practice
✔ When dependency is **required**
✔ Good for immutability and testing

---

# 2️⃣ **Setter Injection**

### **XML-based Setter Injection**

```xml
<bean id="engine" class="com.example.Engine" />

<bean id="car" class="com.example.Car">
    <property name="engine" ref="engine"/>
</bean>
```

### **Car.java**

```java
public class Car {

    private Engine engine;

    public void setEngine(Engine engine) {  // setter injection
        this.engine = engine;
    }
}
```

### When to Use?

✔ When the dependency is **optional**
✔ When dependency may change later

---

# 3️⃣ **Field Injection (Annotation-based only)**

> ❌ Field Injection cannot be done using XML.
> ✔ It can be done using annotations.

### **Car.java**

```java
@Component
public class Car {

    @Autowired
    private Engine engine;   // field injection

    public void drive(){
        engine.start();
    }
}
```

### When to Use?

✔ Quick setups
❌ Not recommended for large projects
❌ Hard to test

---

# 🔥 Full Annotation Example (Spring Without Boot)

### `Engine.java`

```java
@Component
public class Engine {
    public void start() {
        System.out.println("Engine started...");
    }
}
```

### `Car.java`

```java
@Component
public class Car {

    private Engine engine;

    @Autowired
    public Car(Engine engine) {  // constructor DI
        this.engine = engine;
    }

    public void drive() {
        engine.start();
        System.out.println("Car is running...");
    }
}
```

### **applicationContext.xml**

```xml
<context:component-scan base-package="com.example" />
```

---

# 🧠 Summary (for Spring Core)

| DI Type                   | XML Support | Annotation Support | Recommended?    |
| ------------------------- | ----------- | ------------------ | --------------- |
| **Constructor Injection** | ✔ Yes       | ✔ Yes              | ⭐ Best          |
| **Setter Injection**      | ✔ Yes       | ✔ Yes              | Good            |
| **Field Injection**       | ❌ No        | ✔ Yes              | Not recommended |

---

# ✅ Why Constructor Injection Is Best (In Short)

### **1️⃣ Ensures Mandatory Dependencies**

A class **cannot be created without its required dependencies**.
This prevents `NullPointerException` because dependencies are guaranteed.

---

### **2️⃣ Makes the Class Immutable**

All dependencies are set **only once** through the constructor
→ No chance of accidental changes later
→ Immutable = safer & cleaner code

---

### **3️⃣ Easy to Test**

You can easily pass **mock objects** into the constructor in unit tests.
Setter/field injection makes testing harder.

---

### **4️⃣ Encourages Clean Design**

If a constructor has too many dependencies, it signals:
👉 *The class is doing too much*
This naturally promotes **good architecture**.

---

### **5️⃣ Works Best With `final` Keyword**

With constructor injection, you can write:

```java
private final Engine engine;
```

`final` ensures dependency is always initialized — improves reliability.

---

# 🎯 Summary in One Line

**Constructor Injection = safer, cleaner, testable, and mandatory dependency control.**

