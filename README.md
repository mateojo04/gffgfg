 # Solid and clean code in Java
In this repository, I will be documenting the concepts, principles and best practices of Clean code y the SOLID principles in Java. My goal is to have a resource that I can return when I need to review or consult especific examples. here, you'll find examples organized by topics, along with brief explanations for each. 

## 📚 Table of Contents

- [Principles of Clean Code](#principles-of-clean-code)
  - [Names That Reveal Intention](#names-that-reveal-intention)
    - [Names should be clear and reflect our intention](#names-should-be-clear-and-reflect-our-intention)
    - [Avoid misinformation](#avoid-misinformation)
    - [Use pronounceable names](#use-pronounceable-names)
    - [Names of classes and methods](#names-of-classes-and-methods)
    - [Add context that provides meaning](#add-context-that-provides-meaning)
  - [Functions](#functions)
  - [Comments](#comments)
    - [Code smells in commentss](#code-smells-in-comments)
    - [Good comments](#good-comments)
  - [Code Formatting](#code-formatting)
  - [Error Handling](#error-handling)
  - [Unit Testing](#unit-testing)
    - [Test Driven development](#test-driven-development)
  - [Code smells in the development environment](#Code-smells-in-the-development-environment)
  - [Code Smells in Java](#code-smells-in-java)
    - [Very long list of imports](#Very-long-list-of-imports)
    - [Inheriting constants](#inheriting-constants)
    - [Enums vs Constants](#enums-vs-constants)
- [SOLID Principles](#solid-principles)
  - [Cohesion](#cohesion)
  - [Coupling](#coupling)
  - [ Responsibility Principle (SRP)](#-responsibility-principle-srp)
  - [Open/Closed Principle (OCP)](#openclosed-principle-ocp)
  - [Liskov Substitution Principle (LSP)](#liskov-substitution-principle-lsp)
  - [Interface Segregation Principle (ISP)](#interface-segregation-principle-isp)
  - [Dependency Inversion Principle (DIP)](#dependency-inversion-principle-dip)

## Principles of Clean Code
Clean code is easy of read, understand and modify. it's achieved through a clear structured, meaningful names, small functions and ** Responsability per Component**. its quality is maintained and improved through continuous and safe refactoring, supported by a good unit test coverage that allows those changes to be made with confidence.
### Names that reveal intention

#### Names should be clear and reflect our intention.

❌ **Bad example**  
```java
int d;
int m;
int m;
```

✅ **Good example** 
```java
int dayOfBirth;
int monthOfBirth;
int yearOfBirth;
```

Although in loops it is entirely posible and reasonable create short names for variables.
```java
for(int i=0;i< nuumberOfRequests){
    processsRequest(request.get(i);
}
```

#### Avoid misinformation

Avoid calling classes with names that are difficult to look up and name.
```java
public class ServiceForEfficientUpdateMysqlDatabase{
```
```java
public class ServiceForEfficientBackupMysqlDatabase{
```

This is not actually a list, so adding the word List can be confusing.
```java
Map<Integer, String> employeeList;
```

These two classes do not have a clear difference, therefore they can cause confusing.
```java
public class ProductData{}
public class ProductInfo{}
```

#### Use pronounceable names

❌ **Bad example** 
```java
String nn;
String ln;
```
✅ **Good example**
```java
String name;
String lastname;
```
- Avoid using abbreviations 
   
   ❌ **Bad example** 
  ```java
  String lstUsedName;
  ```
  ✅ **Good example**
  ```java
  String leastUsedName;
  ```
- It's much better to choose a very large name than one that doesn't convey it's meaning clearly.

#### Names of classes and methods
- The classes names must be **a name or set of names,** should not be verbs. 
- The methods must be **verbs,** indicating an action.
- Do not name a class ```CreateEmployee{``` or a method ```EmployeeName() ```, for example.

✅ **Good example**
```java
public class DateParser{
    public Date parse(String date);
}
```


#### Add context that provides meaning
```
firstName, lastName, street, houseNumber, city,state, zipcode.
```
These variables clearly form an address when used together, but adding a prefix could provide imformation. For example, if a User class has name but also an address.

This way It becomes clearer what each one is used.
```user.getFirstName()``` **vs** ```user.getAddrFirstName()```
### Functions
- They're very **small.**
- They must do **one thing.**

  ❌ **Bad example**

  We have this function that does two things. violing this principle.
  ```java
  public Integer readNumbersFromFileAndCalculateTotal(String fileUrl) {
    List<String> numbers =
        Files.readAllLines(Paths.get(fileUrl), StandardCharsets.UTF_8);

    Integer total = 0;
    for (String numberString : numbers) {
        total += Integer.valueOf(numberString);
    }

    return total;
  }
  ```
  ✅**Good example**

  Now with the refactored code, you can see that each function does just one thing and does the same thing as the previous one,making it easier to read.
  ```java
  public Integer getMonthlySales() {
    List<String> sales = readNumbersFromFile(MONTHLY_SALES_FILE_PATH);
    return calculateSummatory(sales);
  }

  public List<String> readNumbersFromFile(String fileUrl) {
    return Files.readAllLines(Paths.get(fileUrl), StandardCharsets.UTF_8);
  }

  public Integer calculateSummatory(List<String> values) {
    Integer total = 0;
    for (String numberString : values) {
        total += Integer.valueOf(numberString);
    }

    return total;
  }

  ```


- **Level of abstraction**

  ❌ **Bad example**

  This class performs functions that are not part of its responsabilities. 
  ```java
  public class Person {

    public void drive(Car car) {
        car.openDoor();
        car.setDriver(this);
        car.getBattery().connect();
        car.getEngine().start();
    }
  }
  ```
  ✅ **Goog example**
  
  However, in this version the Person class only handles its functions and the Car class handles its own.
   ```java
   public class Person {

    public void drive(Car car) {
        car.openDoor();
        car.setDriver(this);
        car.start();
    }
  } 
  ```
  ```java
  public class Car {
    Battery battery;
    Engine engine;

    public void start() {
        battery.connect();
        engine.start();
    }
  }
  ```
- They receive **few arguments.**
- They have no side effects.
- Return **exceptions** instead of error codes.

#### Code smell in functions
- **Too many arguments**
  - ⚠️**Avoid the functions with >3 arguments**⚠️
  - Divide the function in functions  smaller or encapsulating the arguments in a class.
- **output arguments:** Avoid passing output arguments in the functions.
  
  ❌ **Bad example**
  ```java
  public void calculateSum(List<Integer> numbers, int result) {
  ```  
  ✅ **Goog example**

  instead it should return the result.
    ```java
  public int calculateSum(List<Integer> numbers) {
  ```  
- **Passing flags in the arguments:** Avoid passing boolean, it often indicates that the function is doing more of one thing. In the next expamle the better is make tow functions to each type item.
   ```
  private static final Double PREMIUM_DISCOUNT_FACTOR = 0.2;
  private static final Double REGULAR_DISCOUNT_FACTOR = 0.1;

   public Double calculateDiscount(Item item, boolean isPremium) {
    if (isPremium) {
        return item.getPrice() * PREMIUM_DISCOUNT_FACTOR;
    }

    return item.getPrice() * REGULAR_DISCOUNT_FACTOR;
  }
   ``` 
- **Died functions:** These are unused functions; these usually indicate that the function does more than one thing. In the following example, it would be best to create two methods for each item type.
### Comments
- Add comments just when strictly necessary.
- They're very **difficult to maintain,** code change a lot and comments become **quickly outdated.**
- If the code has many comments, can be for tow reasons:
  - The code is not understood => Refactoring.
  - The comments are obvious => delete comments.

  ❌ **Bad example**
  ```java
  //Chek if password is  ServiceForEfficientUpdateMysqlDatabase
  if(password != null && password.length > 9 &&!password.contains(username))
  ```
  ✅ **good example**
   ```java
   if(isSecure(password))
   ```

- Never leave code commented out the is no longer being used. **Version control systems make commented-out code unnecessary.**

#### Code smells in comments
- **Inappropiate information:** anything that is better in another system (e.g. versions control system)  
    ```java
     //@Author Mateo Josue
     //Create Date: 10-07-2025
     public class Comments
    ```
- **Redundant comments:** This type of comments not add value because they are too obvious.


    ```java
    /**
    *
    * @param a The first number
    * @param a The second number
    * @return a+b
    */
     public Integer add(Integer a, Integer b){
         return a +b;
     }
    ```
- **Obsolete comments:** Any comment outdated must be updated or deleted, because it not add nothing.

    ```java
     private Date LastLoginDate; //Last login date as String (DD-MM-YYYY HH:MM)
    ```
- **Poorly written comments**
  - Do not make spelling mistakes.
  - Don't comment on obvious things! You must be direct.
  - Make sure you understand.

#### Good comments
Complex regular expressions at a glance.  
```java
//Matches Dates in format dd/mm/yyyy
Pattern.matches("^(0[1-9]|[12][0-9]|3[01])([-/.])(0[1-9]|1[0-2])\\2(\\d{4})$")
```
Comments in critical locations that require code changes.   
```java
//TODO except this metod tochange when the client changes auth API
public void login(User user){
```

_The comments on a public API that **will use a lot of people** are another clear example of good comments._
### Code format
- Configure the IDE to automatically apply formats when saving changes.
- The **entire team must write under the same rules.**
- [Google styles guide](https://google.github.io/styleguide/)
- **Vertical format**. density, order and distance.
  
   ❌ **High density**

  This code has high density aand hinders reading. 
  ~~~~java
  public class Dog {
    String name;
    int age;
    boolean run;
    public Dog(String name, int age, boolean run) {
        this.name = name;
        this.age = age;
        this.run = run;
    }
    public void bark() {
        System.out.println(name  + " says Woof!");
    }
     public void run() {
         this.run=true;
    }
  } 
  ~~~~
  ✅ **Low density**

  It's much better to have code with blank lines separating grouped concepts.
  ```java
  public class Dog {
    String name;
    int age;
    boolean run;

    public Dog(String name, int age, boolean run) {
        this.name = name;
        this.age = age;
        this.run = run;
    }

    public void bark() {
        System.out.println(name + " says Woof!");
    }
    
    public void run() {
         this.run=true;
    }
  } 
  ```
 

  ❌ **Messy code**
  It's difficult to read the code if the functions don't follow an order.
  ```java
  public void c(){
  //...   
  }

  public void b(){
    //...   
  }
 
  public void a(){
     b();
     c();
  }
  ``` 


  ✅ **Ordered code**
  It's much better to have a function at the top, which in turn calls other functions, declared in the order they are invoked.
  ```java
  public void a(){
     b();
     c();
  }
  public void b(){
    //...   
  }
  public void c(){
  //...   
  }

  ```

  
  ❌ **Bad example**
  
  This code isn't separated correctly and hinders reading. 

  ```java
  public String getUserConfigAsString() {

      Session session = getSession();

      User currentUser = session.get(username);
      
      String configAsString = null;

      if (currentUser.isValid()){
       
        Config config = currentUser.getConfig();
        
        configAsString = parseConfig(config);
        } 

        return configAsString;
  }
  
  ```
  ✅ **Good example**
  However, this code does maintain a grouping by related concepts and separates different concepts.
   ```java
  public String getUserConfigAsString() {
      Session session = getSession();
      User currentUser = session.get(username);
      
      String configAsString = null;
      if (currentUser.isValid()){
        Config config = currentUser.getConfig();
        configAsString = parseConfig(config);
        } 

        return configAsString;
  }
  ```
- **Horizontal Format**. Density and indentation.
  
  ❌ **High density**

  This code is very compact
  and contains no spaces, which makes it difficult to read.
  ```java
   public int sumFourNumbers(Integer num1,Integer num2,Integer num3,Integer num4) {
        if (num1!=null&&num2!=null&&num3!=null&&num4!=null) {
            return num1+num2+num3+num4;
        }
        return 0;
  }
  ```

  ✅ **Low density**

  However, this code is more separated, using spaces to separate operators, parameters and conditionals, so it is easier to read.
  ```java
   public int sumFourNumbers(Integer num1, Integer num2, Integer num3, Integer num4) {
        if (num1 != null && num2 != null && num3 != null && num4 != null) {
            return num1 + num2 + num3 + num4;
        } 
        return 0;
  }
  ```


  ❌ **Bad identation**

  The code does not respect the margins and the flow of logic is not clearly visible.
  ```java
  public int sumFourNumbers(Integer num1,Integer num2,Integer num3,Integer num4) {
  if (num1!=null&&num2!=null&&num3!=null&&num4!=null) {
  return num1+num2+num3+num4;
  } 
  return 0;
  }
  ```
  - Always **follow the same identation rules.**
  - Recommended **2 or 4 spaces** of indentation
  - Recommended **maximum 100 characters,** after that amount, make a line break.

### Error Handling
Error handling can make your code very dirty, so it's worth paying attention to.

- Return **exceptions** instead of error codes



    ❌ **Bad example**

    The problem with this code is that they have to control all errors and the correct execution is not clearly seen.

    ```java
    public class DeviceController {

        public void sendShutDown() {
            DeviceHandle handle = getHandle(DEV1);
            //check the state of the device 
            if (handle != DeviceHandle.INVALID) {
                //save the state of the device in the record field
                DeviceRecord record = retrieveDeviceRecord(handle);
                //if not suspend, shut down
                if (record.getStatus() != DEVICE_SUSPENDED) {
                    pauseDevice(handle);
                    clearDeviceWorkQueue(handle);
                    closeDevice(handle);
                }else {
                    logger.log(“Device suspended. Unable to shut down”);
                }
            }else {
                logger.log(“Invalid handle for: ” + DEV1.toString());
            }
        }
    }
    ```

    ✅ **Good example**

    However, in this version, we can see the separation of responsibilities: while one function contains the logic and throws an exception in case of error, the other function handles errors.

    ```java
    public void sendShutDown() {
        try {
            tryToShutDown();
        } catch (DeviceShutDownError e) {
            logger.log(e);
        }
    }

    private void tryToShutDown() throws DeviceShutDownError {
        DeviceHandle handle = getHandle(DEV1);
        DeviceRecord record = retrieveDeviceRecord(handle);
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
    }
    ```

    **Recommendation:** Have the try catch separate from a function, to have both functions working at a  level of responsibility.


- **Unchecked exceptions**
  
  **Unchecked exceptions** are exceptions that inherit from the Exception class and do not require immediate handling. However, **Checked exceptions** require us to throw them or catch them in a try catch block, as in the following code.

    ```java
    public class Driver {
        public void startDriving() {
            try {
                car.start();
            } catch (BatteryFailureException | EngineFailureException e) {
                logger.log("Could not start driving. Car failure: " + e);
            }
        }
    }

    public class Car {
        public void start() throws BatteryFailureException, EngineFailureException {
            battery.connect();
            engine.start();
        }
    }

    public class Battery {
        public void connect() throws BatteryFailureException;
    }

    public class Engine {
        public void start() throws EngineFailureException;
    }

    ```
    In this code, the **Battery** and **Engine** methods throw **Checked exceptions** type exceptions, so each method that invokes them must throw those exceptions or capture them. However, if they were **Unchecked exceptions**, they would only be captured in the **startDriving** method.mbio si fueran **Unchecked exceptions** solo se capturan en el metodo **startDriving**.

- Never return **null**

  ❌ **Bad example**
  
  Returning **null** forces us to always validate it, and if it is not validated every time a method that returns **null** is invoked, a **NullPointerException** would be thrown.**

  ```java
  public void processRequestBatch() {
    List<Request> requestList = getRequestList();
    if (requestList != null) {
        for (Request r : requestList) {
            processRequest(r);
        }
    }
  }

  ```

  ✅ **Good example**

  However, if the ``` getRequestList()``` method returns an empty object instead of **null**, that unnecessary validation is skipped.
  ```java
  public void processRequestBatch() {
    List<Request> requestList = getRequestList();
    for (Request r : requestList) {
            processRequest(r);
        }
  }

  ```
    - Never pass **null**

    ❌ **Bad example**
    
    In this example, the same thing happens as in the previous case, passing null as a parameter forces us to perform unnecessary validations, and if a method does not validate **null**, a **NullPointerException** would occur.**null** ocurriria una **NullPointerException**
    ```java
    public Point getMiddlePoint(Point a, Point b) {
        if (a != null && b != null) {
            Double resultPointX = (a.getX() - b.getX()) / 2;
            Double resultPointY = (a.getY() - b.getY()) / 2;

            return new Point(resultPointX, resultPointY);
        }

        // Return?
    }

    ```

    ✅ **Good example**

    On the other hand, if we do not send **null** as the value of a parameter, there would be no reason to perform that validation, although this case is more difficult than the previous one since we cannot control who is calling our method and how it is being implemented.
     ```java
    public Point getMiddlePoint(Point a, Point b) {
        Double resultPointX = (a.getX() - b.getX()) / 2;
        Double resultPointY = (a.getY() - b.getY()) / 2;

        return new Point(resultPointX, resultPointY);

    }
    ```

### Unit Testing
- Essential for code refactoring -
- Maintaining high coverage **(>90%)** offers great security when making changes to your functions.

#### Test Driven development 

**1.** Write a test,  watch it fail

**2.** Write just enough code to pass the test

**3.** Improve the code without changin its behavior
### Code smells in the development environment
- **Compilation Requires More Than One Step:** You should be able to compile the code in the simplest way possible.
- **Tests require more than one step**
- Tests should be run with a single command
- Danger of **Not running tests regularly out of "laziness"**.


### Code smells in Java
#### Very long list of imports
- If you use **2 or more** classes from a package, import the entire package:

_import package.*_
- We don't want to clutter up our modules with 100 lines of imports.

#### Inheriting constants

 ❌ **Bad example**

In this example, the **EasyGame** class inherits from **Game**, which in turn implements the **GameConstants** interface. Ultimately, the **EasyGame** class inherits that constant, but it's very confusing to do it this way.
  ```java
  public class EasyGame extends Game {
    private int currentLevel;

    public Double calculateProgress() {
        return currentLevel / (double) NUMBER_OF_LEVELS;
    }
  }

  public abstract class Game implements GameConstants {
    public abstract Double calculateProgress();
  }

  public interface GameConstants {
    public static final int NUMBER_OF_LEVELS;
  }

  ```

  ✅ **Good example**

  Instead, it would be recommended to use the constant directly from the class or statically import the classes with the constants.
   ```java
  public Double calculateProgress() {
        return currentLevel / (double) GameConstants.NUMBER_OF_LEVELS;
    }
  ``` 


#### Enums vs Constants
  
  Use enums whenever possible as they provide abstract functions, which save us from having to perform validations when using constants.
  ```java
  public class Game {
    private int currentLevel;
    DifficultyLevel difficultyLevel;

    public int levelsLeft() {
        return difficultyLevel.numberOfLevels() - currentLevel;
    }
  }
  ``` 

  ```java
  public enum DifficultyLevel {
    EASY {
        public int numberOfLevels() {
            return 20;
        }
    },
    MEDIUM {
        public int numberOfLevels() {
            return 30;
        }
    },
    HARD {
        public int numberOfLevels() {
            return 50;
        }
    };

    public abstract int numberOfLevels();
  }
  ``` 
## principles solid
They are a set of principles proposed by Robert C. Marin that will help us:
- Create **scalable software**
- Create a **clean and maintainable architecture**
- Write code that is **easier to read** and understand
- Create modules with high **and loose coupling**
  
### Cohesion 
- It is the degree to which the elements of a module are related.**
- We are interested in a module having a bery high cohesion.

### coupling
- Degree to which two modules are related to each other.
- the modules should have little coupling. 
- if a module is modified, *it should affect the others as little as possible.*

###  responsability principle (SRP)

- A module should have **one reason to change,** not that a module should do one thing.
- A module must be **responsable for a  user of the system.**

❌ **Bad example**
  
In this example, the module would have more than one reason to change.

- If the employee information changes.
- If the way the annual salary is calculated changes.
- If changes need to be made to the report.

```java
public class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public double calculateAnnualSalary() {
        return salary * 12;
    }

    public void printReport() {
        System.out.println("Employee: " + name);
        System.out.println("Annual Salary: " + calculateAnnualSalary());
    }
}
```


 ✅ **Good example**
 
Now this way each module would have only one reason to change.

 ```java
public class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }
}
 ```
```java
public class SalaryCalculator {
    public double calculateAnnualSalary(Employee employee) {
        return employee.getSalary() * 12;
    }
}
```
```java
public class ReportPrinter {
    public void print(Employee employee, double annualSalary) {
        System.out.println("Employee: " + employee.getName());
        System.out.println("Annual Salary: " + annualSalary);
    }
}
```


### Open-close principle(OPC)
- A software artifact must be open for extension but closed for modification.
- We must be able to increase the functionality of a software artifact without modifying existing functionalities. 

❌ **Bad example**

In this expample we have a classe with one function that process payments, the problem that every time a payment is added the function must be modified.
```java
 public class PaymentProcessor {
    public void process(String paymentType) {
        if (paymentType.equals("credit")) {
            System.out.println("Processing credit card payment...");
        } else if (paymentType.equals("paypal")) {
            System.out.println("Processing PayPal payment...");
        }
    }
}
```

✅ **Good example** 

But if we use an interface that allows us to create implementations for different types of payments.

```java
public interface Payment {
    void process();
}

public class CreditCardPayment implements Payment {
    public void process() {
        ....
}

public class PayPalPayment implements Payment {
    public void process() {
        ....
}

```

Now the payment processing class will not be modified and any other payment methods implemented will work.

```java
public class PaymentProcessor {
    public void processPayment(Payment payment) {
        payment.process();
    }
}
```
### Liskov Substitution Principle (LSP)
All classes that inherit from another can be used interchangeably without any problem, including the parent class.

❌ **Bad example**

The **Dog and  Dolphin** classes inherit from **Mammal**, but the **Dolphin** class in the function **Walk** throw an exception because a dolphin doesn't walk, **this behavior violates the principle.** because if a class has an **Mammal** object whatever implementation, it not be cause any problem.
```java
public class Mammal {
    private Integer weight;
    private Integer ageInDays;

    public Integer getWeight() {
        return weight;
    }

    public Integer getAgeInDays() {
        return ageInDays;
    }

    public void walk() {
        System.out.println("I am walking");
    }
    
}

public class Dog extends Mammal {

}

public class Dolphin extends Mammal {

	@Override
	public void walk() {
	   throw new CannotWalkException("I am a dolphin, I cannot walk!");
	}
}
```
✅ **Good example** 

The correct thing to do would be to create a new interface that containing that function and remove it from **Mammal** so that all **Mammal** implementations have the same behavior.

```java
public class landMammal extends Mammal{
	public void walk() {
        System.out.println("I am walking");
    }
}

public class Mammal {
    private Integer weight;
    private Integer ageInDays;

    public Integer getWeight() {
        return weight;
    }

    public Integer getAgeInDays() {
        return ageInDays;
    }
}

public class Dog extends landMammal{

}

public class Dolphin extends Mammal {
  
}

```
### Interface segregation principle (ISP)
Any client must be depend on functions they don't use.

❌ **Bad example**

We have this interface that defines mathematical operations for two calculators, one basic and one advanced.

```java
public interface Operations {

	public Double add(Double a, Double b);

    public Double subtract(Double a, Double b);

    public Double multiply(Double a, Double b);

    public Double divide(Double a, Double b);

    public Double sine(Double angle);

    public Double cosine(Double angle);
}


```
Only the advanced calculator takes of all the functions, while the basic calculator would look something like this.
```java
public class BasicCalculator implements Operations {
   
    /...

    @Override
    public Double sine(Double angle) {
        throw new UnsupportedOperationException(
                "Basic Calculator does not support trigonometric operations");
    }

    @Override
    public Double cosine(Double angle) {
        throw new UnsupportedOperationException(
                "Basic Calculator does not support trigonometric operations");
    }

}

```

✅ **Good example** 

The correct thing is for the basic calculator implementing an interface that only contains the funcitons it needs.

```java
public interface Operations {
    
	public Double add(Double a, Double b);

    public Double subtract(Double a, Double b);

    public Double multiply(Double a, Double b);

    public Double divide(Double a, Double b);

    public Double sine(Double angle);

    public Double cosine(Double angle);
}
```

This new interface would be for the advanced calculator.
```java
public interface TrigonometricOperations {
	public Double sine(Double angle);
    public Double cosine(Double angle);
}

public class AdvancedCalculator implements Operations, TrigonometricOperations {
  //...
}
```

Now it would be like this without those two unnecessary methods.
```java
public class BasicCalculator implements Operations {

    @Override
    public Double add(Double a, Double b) {
        return a + b;
    }

    @Override
    public Double subtract(Double a, Double b) {
        return a - b;
    }

    @Override
    public Double multiply(Double a, Double b) {
        return a * b;
    }

    @Override
    public Double divide(Double a, Double b) {
        return a / b;
    }
}
```




### Dependency inversion principle (DIP)
- The most flexible systems are those that depend on abstractions, not concreteness.
- In Java, a module must depend on interfaces or abstract classes, not volatile implementations.
- This is achieved through mechanisms that create instances of the desired implementations.
- Dependency inversion is expensive.
- ⚠️Analyze whether a module is **volatile** or not⚠️ before abstracting dependencies in this way.

[Example of the dependency inversion principle](https://github.com/MateoRodriguez0/clean-code-java/tree/main/src/main/java/com/clean/code/solid/idp)


