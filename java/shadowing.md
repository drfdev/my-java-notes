### Shadowing

Это концепция когда переменные с один и тем же именем есть как в основном класса так и в его inner-классе
Пример:

```java
// Java program to Demonstrates Shadowing in Java
 
// Class 1 and 2
// Outer Class
class Shadowing {
 
    // Custom instance variable or member variable
    String name = "Outer John";
 
    // Nested inner class
    class innerShadowing {
 
        // Instance variable or member variable
        String name = "Inner John";
 
        // Method of this class to
        // print content of instance variable
        public void print()
        {
 
            // Print statements
            System.out.println(name);
            System.out.println(Shadowing.this.name);
        }
    }
}
 
// Class 3
// Main class
class GFG {
 
    // Main driver method
    public static void main(String[] args)
    {
 
        // Accessing an inner class by
        // creating object of outer class inside main()
        Shadowing obj = new Shadowing();
 
        Shadowing.innerShadowing innerObj
            = obj.new innerShadowing();
 
        // Calling method defined inside inner class
        // inside main() method
        innerObj.print();
    }
}
```

Результат выполнения:
```text
Inner John
Outer John
```

Пример 2

```java
// Java program to demonstrates Shadowing in Java
 
// Class 1 and 2
// Outer Class
class Shadowing {
 
    // Instance variable or member variable
    String name = "Outer John";
 
    // Nested class
    // Inner Class
    class innerShadowing {
 
        // Instance variable or member variable
        String name = "Inner John";
 
        // Method of inner class
        // To print the content
        public void print(String name)
        {
 
            // Print statements
 
            System.out.println(name);
            // This keyword refers to current instance
            // itself
            System.out.println(this.name);
            System.out.println(Shadowing.this.name);
        }
    }
}
 
// Class 3
// Main class
class GFG {
 
    // Main driver method
    public static void main(String[] args)
    {
 
        // Accessing an inner class by
        // creating object of Outer class inside main()
        // method
        Shadowing obj = new Shadowing();
 
        Shadowing.innerShadowing innerObj
            = obj.new innerShadowing();
 
        // Function Call
        innerObj.print("Parameter John");
    }
}
```

Результат выполнения:
```text
Parameter John
Inner John
Outer John
```

---

Info:
- https://www.geeksforgeeks.org/shadowing-in-java/

