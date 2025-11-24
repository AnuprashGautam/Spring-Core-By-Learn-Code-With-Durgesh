# 🌱 What Is Autowiring in Spring?

**Autowiring** is a mechanism in Spring that **automatically identifies and injects the required dependencies** into a bean.

You don’t need to manually write XML or call setters/constructors.
Spring container does all the wiring automatically.

---

# 🔧 Why Autowiring?

Before autowiring (old XML days), you had to write:

```xml
<bean id="paymentService" class="com.app.PaymentServiceImpl"/>

<bean id="orderService" class="com.app.OrderService">
    <constructor-arg ref="paymentService"/>
</bean>
```

With autowiring:

```java
@Autowired
private PaymentService paymentService;
```

Done! Spring injects the correct bean.

---

# 🧠 How Autowiring Works Internally

1. Spring first **creates all beans**.
2. For each bean, Spring looks at fields/constructors/setters.
3. Wherever it finds `@Autowired`, Spring:

   * identifies the **type** of the dependency
   * searches the ApplicationContext
   * if **one matching bean** exists → injects it
   * if **multiple matching beans** exist → you must use `@Qualifier`
   * if **no matching bean** exists → throws error (unless `required=false`)

---

# 🧩 Types / Modes of Autowiring

Autowiring can be used in **three ways** depending on where you place `@Autowired`.

---

# 1️⃣ **Constructor Autowiring (Most recommended)**

If a class has one constructor, Spring automatically injects dependencies even without `@Autowired`.

```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {  // Auto-injected
        this.paymentService = paymentService;
    }
}
```

### ✔ Benefits

* Ensures dependency is mandatory
* Class becomes immutable
* Helps in writing unit tests
* Avoids NullPointerException

This is the **best practice** in modern Spring.

---

# 2️⃣ **Setter / Property Autowiring**

```java
@Service
public class OrderService {

    private PaymentService paymentService;

    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

### ✔ When used?

* When dependency is **optional**
* When you want to allow **modification** after object creation

---

# 3️⃣ **Field Autowiring (Not recommended)**

```java
@Autowired
private PaymentService paymentService;
```

### ❌ Drawbacks

* Not good for testing
* Violates immutability
* Hard to detect null dependencies

Spring still supports it but **constructor injection is strongly preferred**.

---

# 🔎 Autowiring Strategies (How Spring Decides What to Inject)

## **A. By Type (default)**

Spring checks bean type.

If your field is:

```java
@Autowired
private PaymentService paymentService;
```

Spring looks for a bean of type `PaymentService`.

If exactly **one** bean exists → inject it.
If **multiple** beans exist → conflict → use `@Qualifier`.

---

## **B. By Name**

If multiple beans of same type, you specify name:

```java
@Autowired
@Qualifier("onlinePaymentService")
private PaymentService paymentService;
```

Spring will inject the bean with that **name**.

---

## **C. With @Primary**

If multiple beans exist and one should be default:

```java
@Primary
@Service
public class OnlinePaymentService implements PaymentService {}
```

Now Spring uses this unless a qualifier is given.

---

# ⭐ Special Options in Autowiring

### **required=false**

If bean is optional:

```java
@Autowired(required = false)
private DiscountService discountService;
```

Spring will inject it if available; otherwise, skip.

---

### **@Lazy Autowiring**

Loads dependency only when needed.

```java
@Autowired
@Lazy
private HeavyService heavyService;
```

---

# 🏁 Quick Example to Show Everything Together

```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    @Autowired
    public OrderService(
            @Qualifier("onlinePaymentService") PaymentService paymentService
    ) {
        this.paymentService = paymentService;
    }
}
```

Here:

* Constructor injection ✔
* Autowiring ✔
* Qualifier to avoid conflict ✔

This is clean, professional Spring code.

---

# 🧠 Final Summary

Autowiring = Automatic injection of dependencies by Spring.

It works by:

* Scanning beans
* Matching types/names
* Injecting through constructor, setter, or field

Modern Spring best practice:
👉 **Use constructor autowiring + use @Qualifier only when needed.**
