Software Engineering
09-10-2025

### Encapsulation
- Building data, variables, methods, functions, etc. that work into one class. It also hides details from the outside world. 
- Encapsulation is used to controlling the access of information to different parts of your program
- The coding way to think about it is having a private variable that a class or function use even through it does not know what it does. 
- We can think of this as working with a vending machine. All you have to do/know is press a button and we get a drink. We do not have to see all of the wiring and mechanism inside.
```cpp
class BankAccount {
private:
    int balance; // hidden from outside
public:
    void deposit(int amount) { balance += amount; }
    int get_Balance() { return balance; }
};
```
- The balance is hidden, but you can still use deposit/get_Balance safely.
- ---
### Abstraction
- Abstraction is only showing important details and hiding unnecessary ones. It is focusing on what the object does rather than how it does.
- A good analogy is the vending machine from the example above. All you have to do is press a button and we get a drink. We do not have to see all of the wiring and mechanism inside.
```cpp
class Shape {
public:
    virtual void draw() = 0; // abstract function
};

class Circle : public Shape {
public:
    void draw() { cout << "Drawing Circle\n"; }
};
```
You just call `draw()`. You don’t care about the math behind drawing a circle.
- --
### Inheritance
- This is fact that one class can take the properties and behaviors from another class. a "child" inherits from the "parent" class.
- Think of family traits. Just like biology you inherit traits from your parents.
```cpp
class Animal {
public:
    void eat() { cout << "This animal eats food\n"; }
};

class Dog : public Animal {
public:
    void bark() { cout << "Woof!\n"; }
};
```
Dog inherits eat() from animal but can also add its own bark behavior
--- -  
### Polymorphism
- One function or object can take many forms. The exact behavior depends on the object type.
- You can think of this as a smartphone button where the same button can take a photo in camera mode, play/pause in music mode, or hang up in call mode. 
```cpp
class Shape {
public:
    virtual void draw() { cout << "Drawing shape\n"; }
};

class Circle : public Shape {
public:
    void draw() { cout << "Drawing circle\n"; }
};

class Square : public Shape {
public:
    void draw() { cout << "Drawing square\n"; }
};

Shape* s1 = new Circle();
Shape* s2 = new Square();
s1->draw(); // prints "Drawing circle"
s2->draw(); // prints "Drawing square"

```
The same function `draw()` acts differently depending on the object.
- --
### Quick summary
- **Encapsulation** = hide the details inside the “capsule.”    
- **Abstraction** = show only the necessary features.  
- **Inheritance** = pass down traits and behaviors.    
- **Polymorphism** = one thing, many forms.