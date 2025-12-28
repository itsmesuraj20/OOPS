/*
===============================================================================
    COMPLETE OBJECT-ORIENTED PROGRAMMING (OOP) GUIDE - C++
    From Zero to Hero - All Concepts, Patterns & Best Practices
===============================================================================

TABLE OF CONTENTS:
Part 1: OOP Fundamentals (Classes & Objects)
Part 2: The Four Pillars of OOP
Part 3: Advanced OOP Concepts
Part 4: Design Patterns (23 Patterns)
Part 5: SOLID Principles
Part 6: Memory Management & Smart Pointers
Part 7: STL & OOP
Part 8: Interview Questions & Best Practices

===============================================================================
PART 1: OOP FUNDAMENTALS - CLASSES & OBJECTS
===============================================================================
*/

#include <bits/stdc++.h>
using namespace std;

/*
WHAT IS OOP?
- Programming paradigm based on "objects" containing data and methods
- Benefits: Code reusability, modularity, flexibility, maintainability

PROCEDURAL vs OOP:
Procedural: Functions + Data (separate)
OOP: Objects (Data + Functions together)
*/

// ============================================================================
// CONCEPT 1: CLASS & OBJECT BASICS
// ============================================================================

/*
CLASS: Blueprint/Template for creating objects
OBJECT: Instance of a class (actual entity in memory)
*/

// Basic Class Definition
class Student {
public:
    // Data Members (Attributes/Properties)
    string name;
    int age;
    double gpa;
    
    // Member Functions (Methods/Behaviors)
    void display() {
        cout << "Name: " << name << ", Age: " << age << ", GPA: " << gpa << endl;
    }
    
    void study() {
        cout << name << " is studying..." << endl;
    }
};

// Creating Objects
void basicClassExample() {
    // Object creation (Stack allocation)
    Student s1;
    s1.name = "Alice";
    s1.age = 20;
    s1.gpa = 3.8;
    s1.display();
    
    // Object creation (Heap allocation)
    Student* s2 = new Student();
    s2->name = "Bob";
    s2->age = 21;
    s2->gpa = 3.5;
    s2->display();
    delete s2;
}

// ============================================================================
// CONCEPT 2: ACCESS MODIFIERS
// ============================================================================

/*
ACCESS MODIFIERS:
1. PUBLIC: Accessible from anywhere
2. PRIVATE: Accessible only within class
3. PROTECTED: Accessible in class and derived classes

DEFAULT: private for class, public for struct
*/

class BankAccount {
private:  // Can't access from outside
    string accountNumber;
    double balance;
    string pin;
    
protected:  // Accessible in derived classes
    string accountType;
    
public:  // Accessible from anywhere
    string ownerName;
    
    // Public methods to access private data (Encapsulation)
    void setBalance(double bal) {
        if (bal >= 0) balance = bal;
    }
    
    double getBalance() {
        return balance;
    }
    
    void deposit(double amount) {
        if (amount > 0) balance += amount;
    }
    
    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
};

// ============================================================================
// CONCEPT 3: CONSTRUCTORS
// ============================================================================

/*
CONSTRUCTOR: Special method called when object is created
- Same name as class
- No return type
- Used to initialize objects

TYPES:
1. Default Constructor
2. Parameterized Constructor
3. Copy Constructor
4. Constructor Overloading
*/

class Rectangle {
private:
    int length;
    int width;
    
public:
    // 1. Default Constructor
    Rectangle() {
        length = 0;
        width = 0;
        cout << "Default constructor called" << endl;
    }
    
    // 2. Parameterized Constructor
    Rectangle(int l, int w) {
        length = l;
        width = w;
        cout << "Parameterized constructor called" << endl;
    }
    
    // 3. Copy Constructor
    Rectangle(const Rectangle& r) {
        length = r.length;
        width = r.width;
        cout << "Copy constructor called" << endl;
    }
    
    // Constructor with default parameters
    Rectangle(int side) : length(side), width(side) {
        cout << "Square constructor called" << endl;
    }
    
    int area() { return length * width; }
    int perimeter() { return 2 * (length + width); }
    
    void display() {
        cout << "Length: " << length << ", Width: " << width << endl;
    }
};

void constructorExample() {
    Rectangle r1;              // Default
    Rectangle r2(10, 5);       // Parameterized
    Rectangle r3 = r2;         // Copy
    Rectangle r4(7);           // Single parameter
}

// ============================================================================
// CONCEPT 4: DESTRUCTOR
// ============================================================================

/*
DESTRUCTOR: Special method called when object is destroyed
- Same name as class with ~ prefix
- No return type, no parameters
- Used to free resources (memory, files, etc.)
- Called automatically when object goes out of scope
*/

class Resource {
private:
    int* data;
    int size;
    
public:
    Resource(int s) : size(s) {
        data = new int[size];
        cout << "Constructor: Allocated " << size << " integers" << endl;
    }
    
    // Destructor
    ~Resource() {
        delete[] data;
        cout << "Destructor: Freed memory" << endl;
    }
    
    void setData(int index, int value) {
        if (index >= 0 && index < size) data[index] = value;
    }
};

void destructorExample() {
    Resource r(100);  // Constructor called
    r.setData(0, 42);
    // Destructor automatically called when r goes out of scope
}

// ============================================================================
// CONCEPT 5: THIS POINTER
// ============================================================================

/*
THIS POINTER: Pointer to current object
- Available in all non-static member functions
- Used to access object's own members
*/

class Person {
private:
    string name;
    int age;
    
public:
    Person(string name, int age) {
        // Use 'this' to distinguish between parameter and member
        this->name = name;
        this->age = age;
    }
    
    // Return reference to current object (for method chaining)
    Person& setName(string name) {
        this->name = name;
        return *this;
    }
    
    Person& setAge(int age) {
        this->age = age;
        return *this;
    }
    
    void display() {
        cout << "Name: " << this->name << ", Age: " << this->age << endl;
    }
    
    // Compare with another object
    bool isOlderThan(Person& other) {
        return this->age > other.age;
    }
};

void thisPointerExample() {
    Person p1("Alice", 25);
    
    // Method chaining
    p1.setName("Bob").setAge(30).display();
    
    Person p2("Charlie", 20);
    if (p1.isOlderThan(p2)) {
        cout << "p1 is older" << endl;
    }
}

// ============================================================================
// CONCEPT 6: STATIC MEMBERS
// ============================================================================

/*
STATIC MEMBERS: Belong to class, not object
- Shared by all objects
- Only one copy exists
- Access using ClassName::member
*/

class Counter {
private:
    static int count;  // Static data member
    int id;
    
public:
    Counter() {
        count++;
        id = count;
    }
    
    ~Counter() {
        count--;
    }
    
    // Static member function
    static int getCount() {
        return count;
        // Note: Can't access non-static members here
    }
    
    void display() {
        cout << "Object ID: " << id << ", Total objects: " << count << endl;
    }
};

// Static member initialization (outside class)
int Counter::count = 0;

void staticExample() {
    cout << "Initial count: " << Counter::getCount() << endl;
    
    Counter c1, c2, c3;
    cout << "After 3 objects: " << Counter::getCount() << endl;
    
    {
        Counter c4;
        cout << "After 4th object: " << Counter::getCount() << endl;
    }  // c4 destroyed
    
    cout << "After c4 destroyed: " << Counter::getCount() << endl;
}

// ============================================================================
// CONCEPT 7: FRIEND FUNCTION & FRIEND CLASS
// ============================================================================

/*
FRIEND: Can access private and protected members of a class
- Not member of class
- Breaks encapsulation (use sparingly)
*/

class Box {
private:
    int length, width, height;
    
public:
    Box(int l, int w, int h) : length(l), width(w), height(h) {}
    
    // Friend function declaration
    friend int calculateVolume(Box b);
    
    // Friend class declaration
    friend class BoxPrinter;
};

// Friend function definition
int calculateVolume(Box b) {
    // Can access private members
    return b.length * b.width * b.height;
}

// Friend class
class BoxPrinter {
public:
    void printDimensions(Box b) {
        // Can access private members of Box
        cout << "Dimensions: " << b.length << "x" << b.width << "x" << b.height << endl;
    }
};

/*
===============================================================================
PART 2: THE FOUR PILLARS OF OOP
===============================================================================
*/

// ============================================================================
// PILLAR 1: ENCAPSULATION
// ============================================================================

/*
ENCAPSULATION: Bundling data and methods together
- Hide internal details (data hiding)
- Provide public interface
- Use private data + public getters/setters
*/

class Employee {
private:
    // Private data (hidden from outside)
    string name;
    int id;
    double salary;
    string department;
    
public:
    // Constructor
    Employee(string n, int i, double s, string d) 
        : name(n), id(i), salary(s), department(d) {}
    
    // Getters (Read access)
    string getName() const { return name; }
    int getId() const { return id; }
    double getSalary() const { return salary; }
    string getDepartment() const { return department; }
    
    // Setters (Write access with validation)
    void setName(string n) {
        if (!n.empty()) name = n;
    }
    
    void setSalary(double s) {
        if (s > 0) salary = s;
    }
    
    void setDepartment(string d) {
        if (!d.empty()) department = d;
    }
    
    // Business logic (encapsulated)
    void giveRaise(double percentage) {
        if (percentage > 0 && percentage <= 50) {
            salary += salary * (percentage / 100.0);
        }
    }
    
    void display() const {
        cout << "ID: " << id << ", Name: " << name 
             << ", Salary: $" << salary << ", Dept: " << department << endl;
    }
};

// ============================================================================
// PILLAR 2: INHERITANCE
// ============================================================================

/*
INHERITANCE: Create new class from existing class
- Parent/Base/Super class
- Child/Derived/Sub class
- "IS-A" relationship

TYPES:
1. Single Inheritance
2. Multiple Inheritance
3. Multilevel Inheritance
4. Hierarchical Inheritance
5. Hybrid Inheritance
*/

// === SINGLE INHERITANCE ===
class Vehicle {  // Base class
protected:
    string brand;
    int year;
    double price;
    
public:
    Vehicle(string b, int y, double p) : brand(b), year(y), price(p) {}
    
    void start() { cout << brand << " is starting..." << endl; }
    void stop() { cout << brand << " is stopping..." << endl; }
    
    virtual void display() {  // Virtual for polymorphism
        cout << "Brand: " << brand << ", Year: " << year 
             << ", Price: $" << price << endl;
    }
};

class Car : public Vehicle {  // Derived class
private:
    int doors;
    string fuelType;
    
public:
    Car(string b, int y, double p, int d, string f) 
        : Vehicle(b, y, p), doors(d), fuelType(f) {}
    
    void display() override {
        Vehicle::display();  // Call base class method
        cout << "Doors: " << doors << ", Fuel: " << fuelType << endl;
    }
    
    void openTrunk() { cout << "Trunk opened" << endl; }
};

// === MULTILEVEL INHERITANCE ===
class SportsCar : public Car {  // Derived from Car
private:
    int topSpeed;
    
public:
    SportsCar(string b, int y, double p, int d, string f, int speed)
        : Car(b, y, p, d, f), topSpeed(speed) {}
    
    void display() override {
        Car::display();
        cout << "Top Speed: " << topSpeed << " mph" << endl;
    }
    
    void turboBoost() { cout << "Turbo activated!" << endl; }
};

// === MULTIPLE INHERITANCE ===
class GPS {
public:
    void navigate() { cout << "Navigating..." << endl; }
};

class MediaPlayer {
public:
    void playMusic() { cout << "Playing music..." << endl; }
};

class SmartCar : public Car, public GPS, public MediaPlayer {
public:
    SmartCar(string b, int y, double p, int d, string f)
        : Car(b, y, p, d, f) {}
    
    void autoPilot() { cout << "Auto-pilot enabled" << endl; }
};

// === HIERARCHICAL INHERITANCE ===
class Bike : public Vehicle {
private:
    bool hasCarrier;
    
public:
    Bike(string b, int y, double p, bool carrier)
        : Vehicle(b, y, p), hasCarrier(carrier) {}
    
    void wheelie() { cout << "Doing a wheelie!" << endl; }
};

class Truck : public Vehicle {
private:
    int loadCapacity;
    
public:
    Truck(string b, int y, double p, int capacity)
        : Vehicle(b, y, p), loadCapacity(capacity) {}
    
    void loadCargo() { cout << "Loading cargo..." << endl; }
};

// ============================================================================
// PILLAR 3: POLYMORPHISM
// ============================================================================

/*
POLYMORPHISM: "Many forms" - same interface, different implementations

TYPES:
1. Compile-time (Static) Polymorphism
   - Function Overloading
   - Operator Overloading
2. Runtime (Dynamic) Polymorphism
   - Virtual Functions
   - Function Overriding
*/

// === COMPILE-TIME POLYMORPHISM ===

// 1. Function Overloading (Same name, different parameters)
class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }
    
    double add(double a, double b) {
        return a + b;
    }
    
    int add(int a, int b, int c) {
        return a + b + c;
    }
    
    string add(string a, string b) {
        return a + b;
    }
};

// 2. Operator Overloading
class Complex {
private:
    double real, imag;
    
public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // Overload + operator
    Complex operator+(const Complex& c) {
        return Complex(real + c.real, imag + c.imag);
    }
    
    // Overload - operator
    Complex operator-(const Complex& c) {
        return Complex(real - c.real, imag - c.imag);
    }
    
    // Overload * operator
    Complex operator*(const Complex& c) {
        return Complex(real * c.real - imag * c.imag,
                      real * c.imag + imag * c.real);
    }
    
    // Overload == operator
    bool operator==(const Complex& c) {
        return (real == c.real && imag == c.imag);
    }
    
    // Overload << operator (friend function)
    friend ostream& operator<<(ostream& os, const Complex& c);
    
    // Overload >> operator
    friend istream& operator>>(istream& is, Complex& c);
};

ostream& operator<<(ostream& os, const Complex& c) {
    os << c.real << " + " << c.imag << "i";
    return os;
}

istream& operator>>(istream& is, Complex& c) {
    is >> c.real >> c.imag;
    return is;
}

// === RUNTIME POLYMORPHISM ===

// Virtual Functions & Overriding
class Shape {  // Abstract base class
protected:
    string color;
    
public:
    Shape(string c) : color(c) {}
    
    // Virtual function
    virtual double area() {
        return 0;
    }
    
    virtual double perimeter() {
        return 0;
    }
    
    // Pure virtual function (makes class abstract)
    virtual void draw() = 0;
    
    virtual ~Shape() {}  // Virtual destructor
};

class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(string c, double r) : Shape(c), radius(r) {}
    
    double area() override {
        return 3.14159 * radius * radius;
    }
    
    double perimeter() override {
        return 2 * 3.14159 * radius;
    }
    
    void draw() override {
        cout << "Drawing a " << color << " circle" << endl;
    }
};

class RectangleShape : public Shape {
private:
    double length, width;
    
public:
    RectangleShape(string c, double l, double w) 
        : Shape(c), length(l), width(w) {}
    
    double area() override {
        return length * width;
    }
    
    double perimeter() override {
        return 2 * (length + width);
    }
    
    void draw() override {
        cout << "Drawing a " << color << " rectangle" << endl;
    }
};

class Triangle : public Shape {
private:
    double a, b, c;
    
public:
    Triangle(string col, double x, double y, double z)
        : Shape(col), a(x), b(y), c(z) {}
    
    double area() override {
        double s = (a + b + c) / 2;
        return sqrt(s * (s-a) * (s-b) * (s-c));
    }
    
    double perimeter() override {
        return a + b + c;
    }
    
    void draw() override {
        cout << "Drawing a " << color << " triangle" << endl;
    }
};

// Polymorphism in action
void polymorphismDemo() {
    // Base class pointer/reference can hold derived class objects
    Shape* shapes[3];
    shapes[0] = new Circle("Red", 5);
    shapes[1] = new RectangleShape("Blue", 4, 6);
    shapes[2] = new Triangle("Green", 3, 4, 5);
    
    for (int i = 0; i < 3; i++) {
        shapes[i]->draw();
        cout << "Area: " << shapes[i]->area() << endl;
        cout << "Perimeter: " << shapes[i]->perimeter() << endl;
        cout << endl;
    }
    
    // Clean up
    for (int i = 0; i < 3; i++) delete shapes[i];
}

// ============================================================================
// PILLAR 4: ABSTRACTION
// ============================================================================

/*
ABSTRACTION: Hide implementation details, show only essential features
- Abstract classes (can't instantiate)
- Pure virtual functions
- Interfaces (all pure virtual functions)
*/

// Interface example (pure abstract class)
class PaymentProcessor {
public:
    virtual bool processPayment(double amount) = 0;
    virtual bool refund(double amount) = 0;
    virtual string getPaymentStatus() = 0;
    virtual ~PaymentProcessor() {}
};

class CreditCardPayment : public PaymentProcessor {
private:
    string cardNumber;
    string cvv;
    
public:
    CreditCardPayment(string card, string c) : cardNumber(card), cvv(c) {}
    
    bool processPayment(double amount) override {
        cout << "Processing credit card payment of $" << amount << endl;
        // Payment logic here
        return true;
    }
    
    bool refund(double amount) override {
        cout << "Refunding $" << amount << " to credit card" << endl;
        return true;
    }
    
    string getPaymentStatus() override {
        return "Credit Card Payment Successful";
    }
};

class PayPalPayment : public PaymentProcessor {
private:
    string email;
    
public:
    PayPalPayment(string e) : email(e) {}
    
    bool processPayment(double amount) override {
        cout << "Processing PayPal payment of $" << amount << endl;
        return true;
    }
    
    bool refund(double amount) override {
        cout << "Refunding $" << amount << " to PayPal account" << endl;
        return true;
    }
    
    string getPaymentStatus() override {
        return "PayPal Payment Successful";
    }
};

// Using abstraction
void makePayment(PaymentProcessor* processor, double amount) {
    if (processor->processPayment(amount)) {
        cout << processor->getPaymentStatus() << endl;
    }
}

/*
===============================================================================
PART 3: ADVANCED OOP CONCEPTS
===============================================================================
*/

// ============================================================================
// CONCEPT 1: CONST CORRECTNESS
// ============================================================================

class ConstExample {
private:
    int value;
    mutable int accessCount;  // Can be modified in const functions
    
public:
    ConstExample(int v) : value(v), accessCount(0) {}
    
    // Const member function (can't modify member variables)
    int getValue() const {
        accessCount++;  // OK because mutable
        return value;
    }
    
    // Non-const member function
    void setValue(int v) {
        value = v;
    }
    
    // Const object can only call const functions
    void display() const {
        cout << "Value: " << value << endl;
    }
};

// ============================================================================
// CONCEPT 2: COMPOSITION vs AGGREGATION
// ============================================================================

/*
COMPOSITION: "HAS-A" relationship (strong ownership)
- Part cannot exist without whole
- Whole responsible for creation/destruction

AGGREGATION: "HAS-A" relationship (weak ownership)
- Part can exist independently
- Whole not responsible for lifecycle
*/

// COMPOSITION Example
class Engine {
private:
    int horsepower;
    
public:
    Engine(int hp) : horsepower(hp) {
        cout << "Engine created" << endl;
    }
    
    ~Engine() {
        cout << "Engine destroyed" << endl;
    }
    
    void start() { cout << "Engine started" << endl; }
};

class CarWithComposition {
private:
    Engine engine;  // Engine is part of Car (composition)
    string model;
    
public:
    CarWithComposition(string m, int hp) : model(m), engine(hp) {
        cout << "Car created" << endl;
    }
    
    ~CarWithComposition() {
        cout << "Car destroyed" << endl;
    }
    // Engine automatically destroyed when Car is destroyed
};

// AGGREGATION Example
class Department {
private:
    string name;
    
public:
    Department(string n) : name(n) {}
    string getName() { return name; }
};

class University {
private:
    string name;
    vector<Department*> departments;  // Aggregation (weak reference)
    
public:
    University(string n) : name(n) {}
    
    void addDepartment(Department* dept) {
        departments.push_back(dept);
    }
    
    // University doesn't own departments, so doesn't delete them
};

// ============================================================================
// CONCEPT 3: FUNCTION OVERRIDING RULES
// ============================================================================

class Base {
public:
    virtual void func1() { cout << "Base::func1()" << endl; }
    virtual void func2() const { cout << "Base::func2() const" << endl; }
    virtual void func3(int x) { cout << "Base::func3(int)" << endl; }
    void func4() { cout << "Base::func4() non-virtual" << endl; }
};

class Derived : public Base {
public:
    // Correct override
    void func1() override { cout << "Derived::func1()" << endl; }
    
    // Correct override (const matches)
    void func2() const override { cout << "Derived::func2() const" << endl; }
    
    // Correct override (parameter matches)
    void func3(int x) override { cout << "Derived::func3(int)" << endl; }
    
    // This hides Base::func4, doesn't override (not virtual in base)
    void func4() { cout << "Derived::func4()" << endl; }
};

// ============================================================================
// CONCEPT 4: VIRTUAL DESTRUCTOR
// ============================================================================

/*
VIRTUAL DESTRUCTOR: Essential when using polymorphism
- Without virtual destructor: only base class destructor called
- With virtual destructor: proper cleanup of derived class
*/

class BaseWithVirtual {
public:
    BaseWithVirtual() { cout << "Base constructor" << endl; }
    virtual ~BaseWithVirtual() { cout << "Base destructor" << endl; }
};

class DerivedWithVirtual : public BaseWithVirtual {
private:
    int* data;
    
public:
    DerivedWithVirtual() {
        data = new int[100];
        cout << "Derived constructor" << endl;
    }
    
    ~DerivedWithVirtual() {
        delete[] data;
        cout << "Derived destructor" << endl;
    }
};

void virtualDestructorDemo() {
    BaseWithVirtual* ptr = new DerivedWithVirtual();
    delete ptr;  // Calls both destructors (because virtual)
    // Without virtual: only Base destructor called (memory leak!)
}

// ============================================================================
// CONCEPT 5: ABSTRACT FACTORY PATTERN
// ============================================================================

// Abstract Product
class Button {
public:
    virtual void render() = 0;
    virtual void onClick() = 0;
    virtual ~Button() {}
};

class Checkbox {
public:
    virtual void render() = 0;
    virtual void toggle() = 0;
    virtual ~Checkbox() {}
};

// Concrete Products - Windows
class WindowsButton : public Button {
public:
    void render() override { cout << "Rendering Windows button" << endl; }
    void onClick() override { cout << "Windows button clicked" << endl; }
};

class WindowsCheckbox : public Checkbox {
public:
    void render() override { cout << "Rendering Windows checkbox" << endl; }
    void toggle() override { cout << "Windows checkbox toggled" << endl; }
};

// Concrete Products - Mac
class MacButton : public Button {
public:
    void render() override { cout << "Rendering Mac button" << endl; }
    void onClick() override { cout << "Mac button clicked" << endl; }
};

class MacCheckbox : public Checkbox {
public:
    void render() override { cout << "Rendering Mac checkbox" << endl; }
    void toggle() override { cout << "Mac checkbox toggled" << endl; }
};

// Abstract Factory
class GUIFactory {
public:
    virtual Button* createButton() = 0;
    virtual Checkbox* createCheckbox() = 0;
    virtual ~GUIFactory() {}
};

// Concrete Factories
class WindowsFactory : public GUIFactory {
public:
    Button* createButton() override { return new WindowsButton(); }
    Checkbox* createCheckbox() override { return new WindowsCheckbox(); }
};

class MacFactory : public GUIFactory {
public:
    Button* createButton() override { return new MacButton(); }
    Checkbox* createCheckbox() override { return new MacCheckbox(); }
};

/*
===============================================================================
PART 4: DESIGN PATTERNS (23 Gang of Four Patterns)
===============================================================================
*/

// ============================================================================
// CREATIONAL PATTERNS (5)
// ============================================================================

// === 1. SINGLETON PATTERN ===
/*
PURPOSE: Ensure only one instance of class exists
USE CASE: Database connections, loggers, configuration managers
*/

class Singleton {
private:
    static Singleton* instance;
    int data;
    
    // Private constructor
    Singleton() : data(0) {
        cout << "Singleton created" << endl;
    }
    
    // Delete copy constructor and assignment operator
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
    
public:
    static Singleton* getInstance() {
        if (instance == nullptr) {
            instance = new Singleton();
        }
        return instance;
    }
    
    void setData(int d) { data = d; }
    int getData() { return data; }
};

Singleton* Singleton::instance = nullptr;

// Thread-safe Singleton (C++11)
class ThreadSafeSingleton {
private:
    ThreadSafeSingleton() {}
    
public:
    static ThreadSafeSingleton& getInstance() {
        static ThreadSafeSingleton instance;  // Thread-safe in C++11
        return instance;
    }
    
    ThreadSafeSingleton(const ThreadSafeSingleton&) = delete;
    ThreadSafeSingleton& operator=(const ThreadSafeSingleton&) = delete;
};

// === 2. FACTORY PATTERN ===
/*
PURPOSE: Create objects without specifying exact class
USE CASE: When object creation logic is complex
*/

class Animal {
public:
    virtual void makeSound() = 0;
    virtual ~Animal() {}
};

class Dog : public Animal {
public:
    void makeSound() override { cout << "Woof!" << endl; }
};

class Cat : public Animal {
public:
    void makeSound() override { cout << "Meow!" << endl; }
};

class Bird : public Animal {
public:
    void makeSound() override { cout << "Tweet!" << endl; }
};

// Factory
class AnimalFactory {
public:
    static Animal* createAnimal(string type) {
        if (type == "dog") return new Dog();
        if (type == "cat") return new Cat();
        if (type == "bird") return new Bird();
        return nullptr;
    }
};

// === 3. BUILDER PATTERN ===
/*
PURPOSE: Construct complex objects step by step
USE CASE: Objects with many optional parameters
*/

class Pizza {
private:
    string dough;
    string sauce;
    string topping;
    bool cheese;
    
public:
    Pizza() : cheese(false) {}
    
    void setDough(string d) { dough = d; }
    void setSauce(string s) { sauce = s; }
    void setTopping(string t) { topping = t; }
    void setCheese(bool c) { cheese = c; }
    
    void show() {
        cout << "Pizza with: " << dough << " dough, " << sauce 
             << " sauce, " << topping << " topping";
        if (cheese) cout << ", extra cheese";
        cout << endl;
    }
};

// Builder
class PizzaBuilder {
private:
    Pizza* pizza;
    
public:
    PizzaBuilder() { pizza = new Pizza(); }
    
    PizzaBuilder& addDough(string dough) {
        pizza->setDough(dough);
        return *this;
    }
    
    PizzaBuilder& addSauce(string sauce) {
        pizza->setSauce(sauce);
        return *this;
    }
    
    PizzaBuilder& addTopping(string topping) {
        pizza->setTopping(topping);
        return *this;
    }
    
    PizzaBuilder& addCheese() {
        pizza->setCheese(true);
        return *this;
    }
    
    Pizza* build() {
        return pizza;
    }
};

void builderDemo() {
    Pizza* myPizza = PizzaBuilder()
        .addDough("thin crust")
        .addSauce("tomato")
        .addTopping("pepperoni")
        .addCheese()
        .build();
    
    myPizza->show();
}

// === 4. PROTOTYPE PATTERN ===
/*
PURPOSE: Clone objects instead of creating new ones
USE CASE: When object creation is expensive
*/

class Prototype {
public:
    virtual Prototype* clone() = 0;
    virtual void show() = 0;
    virtual ~Prototype() {}
};

class ConcretePrototype : public Prototype {
private:
    int id;
    string name;
    vector<int> data;
    
public:
    ConcretePrototype(int i, string n) : id(i), name(n) {
        // Expensive initialization
        for (int j = 0; j < 1000; j++) data.push_back(j);
    }
    
    Prototype* clone() override {
        return new ConcretePrototype(*this);  // Uses copy constructor
    }
    
    void show() override {
        cout << "ID: " << id << ", Name: " << name << endl;
    }
    
    void setId(int i) { id = i; }
};

// === 5. OBJECT POOL PATTERN ===
/*
PURPOSE: Reuse expensive objects instead of creating new ones
USE CASE: Database connections, thread pools
*/

class Reusable {
private:
    int id;
    
public:
    Reusable(int i) : id(i) {}
    int getId() { return id; }
    void reset() { cout << "Object " << id << " reset" << endl; }
};

class ObjectPool {
private:
    vector<Reusable*> available;
    vector<Reusable*> inUse;
    int nextId;
    
public:
    ObjectPool() : nextId(0) {}
    
    Reusable* acquire() {
        if (available.empty()) {
            Reusable* obj = new Reusable(nextId++);
            inUse.push_back(obj);
            return obj;
        }
        
        Reusable* obj = available.back();
        available.pop_back();
        inUse.push_back(obj);
        return obj;
    }
    
    void release(Reusable* obj) {
        auto it = find(inUse.begin(), inUse.end(), obj);
        if (it != inUse.end()) {
            inUse.erase(it);
            obj->reset();
            available.push_back(obj);
        }
    }
    
    ~ObjectPool() {
        for (auto obj : available) delete obj;
        for (auto obj : inUse) delete obj;
    }
};

// ============================================================================
// STRUCTURAL PATTERNS (7)
// ============================================================================

// === 6. ADAPTER PATTERN ===
/*
PURPOSE: Make incompatible interfaces work together
USE CASE: Integrating legacy code with new systems
*/

// Old interface
class OldPrinter {
public:
    void printOldWay(string text) {
        cout << "[Old Printer] " << text << endl;
    }
};

// New interface
class NewPrinter {
public:
    virtual void print(string text) = 0;
    virtual ~NewPrinter() {}
};

// Adapter
class PrinterAdapter : public NewPrinter {
private:
    OldPrinter* oldPrinter;
    
public:
    PrinterAdapter(OldPrinter* op) : oldPrinter(op) {}
    
    void print(string text) override {
        oldPrinter->printOldWay(text);  // Adapting the interface
    }
};

// === 7. BRIDGE PATTERN ===
/*
PURPOSE: Decouple abstraction from implementation
USE CASE: When both abstraction and implementation vary
*/

// Implementation
class DrawAPI {
public:
    virtual void drawCircle(int x, int y, int radius) = 0;
    virtual ~DrawAPI() {}
};

class DrawAPI1 : public DrawAPI {
public:
    void drawCircle(int x, int y, int radius) override {
        cout << "API1: Drawing circle at (" << x << "," << y 
             << ") radius " << radius << endl;
    }
};

class DrawAPI2 : public DrawAPI {
public:
    void drawCircle(int x, int y, int radius) override {
        cout << "API2: Drawing circle at (" << x << "," << y 
             << ") radius " << radius << endl;
    }
};

// Abstraction
class ShapeBridge {
protected:
    DrawAPI* drawAPI;
    
public:
    ShapeBridge(DrawAPI* api) : drawAPI(api) {}
    virtual void draw() = 0;
    virtual ~ShapeBridge() {}
};

class CircleBridge : public ShapeBridge {
private:
    int x, y, radius;
    
public:
    CircleBridge(int x, int y, int r, DrawAPI* api)
        : ShapeBridge(api), x(x), y(y), radius(r) {}
    
    void draw() override {
        drawAPI->drawCircle(x, y, radius);
    }
};

// === 8. COMPOSITE PATTERN ===
/*
PURPOSE: Treat individual objects and compositions uniformly
USE CASE: Tree structures, file systems
*/

class Component {
protected:
    string name;
    
public:
    Component(string n) : name(n) {}
    virtual void display(int depth = 0) = 0;
    virtual ~Component() {}
};

class Leaf : public Component {
public:
    Leaf(string n) : Component(n) {}
    
    void display(int depth = 0) override {
        cout << string(depth, '-') << name << endl;
    }
};

class Composite : public Component {
private:
    vector<Component*> children;
    
public:
    Composite(string n) : Component(n) {}
    
    void add(Component* c) {
        children.push_back(c);
    }
    
    void remove(Component* c) {
        children.erase(remove(children.begin(), children.end(), c), 
                      children.end());
    }
    
    void display(int depth = 0) override {
        cout << string(depth, '-') << name << endl;
        for (auto child : children) {
            child->display(depth + 2);
        }
    }
    
    ~Composite() {
        for (auto child : children) delete child;
    }
};

// === 9. DECORATOR PATTERN ===
/*
PURPOSE: Add functionality to objects dynamically
USE CASE: Adding features without modifying original class
*/

class Coffee {
public:
    virtual string getDescription() = 0;
    virtual double cost() = 0;
    virtual ~Coffee() {}
};

class SimpleCoffee : public Coffee {
public:
    string getDescription() override { return "Simple Coffee"; }
    double cost() override { return 5.0; }
};

// Decorator
class CoffeeDecorator : public Coffee {
protected:
    Coffee* coffee;
    
public:
    CoffeeDecorator(Coffee* c) : coffee(c) {}
    virtual ~CoffeeDecorator() { delete coffee; }
};

class MilkDecorator : public CoffeeDecorator {
public:
    MilkDecorator(Coffee* c) : CoffeeDecorator(c) {}
    
    string getDescription() override {
        return coffee->getDescription() + ", Milk";
    }
    
    double cost() override {
        return coffee->cost() + 1.5;
    }
};

class SugarDecorator : public CoffeeDecorator {
public:
    SugarDecorator(Coffee* c) : CoffeeDecorator(c) {}
    
    string getDescription() override {
        return coffee->getDescription() + ", Sugar";
    }
    
    double cost() override {
        return coffee->cost() + 0.5;
    }
};

void decoratorDemo() {
    Coffee* coffee = new SimpleCoffee();
    coffee = new MilkDecorator(coffee);
    coffee = new SugarDecorator(coffee);
    
    cout << coffee->getDescription() << " costs $" << coffee->cost() << endl;
    delete coffee;
}

// === 10. FACADE PATTERN ===
/*
PURPOSE: Provide simplified interface to complex subsystem
USE CASE: Simplifying library usage
*/

// Complex subsystems
class CPU {
public:
    void freeze() { cout << "CPU: Freezing..." << endl; }
    void jump(long position) { cout << "CPU: Jumping to " << position << endl; }
    void execute() { cout << "CPU: Executing..." << endl; }
};

class Memory {
public:
    void load(long position, string data) {
        cout << "Memory: Loading data at " << position << endl;
    }
};

class HardDrive {
public:
    string read(long lba, int size) {
        cout << "HardDrive: Reading " << size << " bytes from " << lba << endl;
        return "data";
    }
};

// Facade
class ComputerFacade {
private:
    CPU* cpu;
    Memory* memory;
    HardDrive* hardDrive;
    
public:
    ComputerFacade() {
        cpu = new CPU();
        memory = new Memory();
        hardDrive = new HardDrive();
    }
    
    void start() {
        cout << "Computer starting..." << endl;
        cpu->freeze();
        memory->load(0, hardDrive->read(0, 1024));
        cpu->jump(0);
        cpu->execute();
        cout << "Computer started!" << endl;
    }
    
    ~ComputerFacade() {
        delete cpu;
        delete memory;
        delete hardDrive;
    }
};

// === 11. FLYWEIGHT PATTERN ===
/*
PURPOSE: Share common data to save memory
USE CASE: Large numbers of similar objects
*/

class Character {
private:
    char symbol;
    string font;
    int size;
    
public:
    Character(char s, string f, int sz) : symbol(s), font(f), size(sz) {}
    
    void display(int row, int col) {
        cout << "Displaying '" << symbol << "' at (" << row << "," << col 
             << ") font:" << font << " size:" << size << endl;
    }
};

class CharacterFactory {
private:
    map<string, Character*> characters;
    
public:
    Character* getCharacter(char symbol, string font, int size) {
        string key = string(1, symbol) + font + to_string(size);
        
        if (characters.find(key) == characters.end()) {
            characters[key] = new Character(symbol, font, size);
        }
        
        return characters[key];
    }
    
    ~CharacterFactory() {
        for (auto& pair : characters) delete pair.second;
    }
};

// === 12. PROXY PATTERN ===
/*
PURPOSE: Control access to an object
USE CASE: Lazy loading, access control, logging
*/

class Image {
public:
    virtual void display() = 0;
    virtual ~Image() {}
};

class RealImage : public Image {
private:
    string filename;
    
    void loadFromDisk() {
        cout << "Loading image: " << filename << endl;
        // Expensive operation
    }
    
public:
    RealImage(string file) : filename(file) {
        loadFromDisk();
    }
    
    void display() override {
        cout << "Displaying image: " << filename << endl;
    }
};

class ProxyImage : public Image {
private:
    string filename;
    RealImage* realImage;
    
public:
    ProxyImage(string file) : filename(file), realImage(nullptr) {}
    
    void display() override {
        if (realImage == nullptr) {
            realImage = new RealImage(filename);  // Lazy loading
        }
        realImage->display();
    }
    
    ~ProxyImage() {
        if (realImage) delete realImage;
    }
};

// ============================================================================
// BEHAVIORAL PATTERNS (11)
// ============================================================================

// === 13. OBSERVER PATTERN ===
/*
PURPOSE: Notify multiple objects about state changes
USE CASE: Event handling, MVC pattern
*/

class Observer {
public:
    virtual void update(float temperature) = 0;
    virtual ~Observer() {}
};

class Subject {
private:
    vector<Observer*> observers;
    float temperature;
    
public:
    void attach(Observer* obs) {
        observers.push_back(obs);
    }
    
    void detach(Observer* obs) {
        observers.erase(remove(observers.begin(), observers.end(), obs), 
                       observers.end());
    }
    
    void setTemperature(float temp) {
        temperature = temp;
        notify();
    }
    
    void notify() {
        for (auto obs : observers) {
            obs->update(temperature);
        }
    }
};

class Display : public Observer {
private:
    string name;
    
public:
    Display(string n) : name(n) {}
    
    void update(float temperature) override {
        cout << name << " display: Temperature is " << temperature << "°C" << endl;
    }
};

// === 14. STRATEGY PATTERN ===
/*
PURPOSE: Define family of algorithms, make them interchangeable
USE CASE: Different sorting algorithms, payment methods
*/

class SortStrategy {
public:
    virtual void sort(vector<int>& arr) = 0;
    virtual ~SortStrategy() {}
};

class BubbleSort : public SortStrategy {
public:
    void sort(vector<int>& arr) override {
        cout << "Sorting using Bubble Sort" << endl;
        // Bubble sort implementation
    }
};

class QuickSortStrategy : public SortStrategy {
public:
    void sort(vector<int>& arr) override {
        cout << "Sorting using Quick Sort" << endl;
        // Quick sort implementation
    }
};

class Sorter {
private:
    SortStrategy* strategy;
    
public:
    Sorter(SortStrategy* s) : strategy(s) {}
    
    void setStrategy(SortStrategy* s) {
        strategy = s;
    }
    
    void sort(vector<int>& arr) {
        strategy->sort(arr);
    }
};

// === 15. COMMAND PATTERN ===
/*
PURPOSE: Encapsulate request as object
USE CASE: Undo/Redo, macro recording
*/

class Command {
public:
    virtual void execute() = 0;
    virtual void undo() = 0;
    virtual ~Command() {}
};

class Light {
public:
    void on() { cout << "Light is ON" << endl; }
    void off() { cout << "Light is OFF" << endl; }
};

class LightOnCommand : public Command {
private:
    Light* light;
    
public:
    LightOnCommand(Light* l) : light(l) {}
    
    void execute() override {
        light->on();
    }
    
    void undo() override {
        light->off();
    }
};

class RemoteControl {
private:
    Command* command;
    vector<Command*> history;
    
public:
    void setCommand(Command* cmd) {
        command = cmd;
    }
    
    void pressButton() {
        command->execute();
        history.push_back(command);
    }
    
    void pressUndo() {
        if (!history.empty()) {
            history.back()->undo();
            history.pop_back();
        }
    }
};

// === 16. TEMPLATE METHOD PATTERN ===
/*
PURPOSE: Define skeleton of algorithm, let subclasses override steps
USE CASE: Frameworks, game engines
*/

class DataMiner {
public:
    // Template method
    void mine() {
        openFile();
        extractData();
        parseData();
        analyzeData();
        closeFile();
    }
    
protected:
    virtual void openFile() = 0;
    virtual void extractData() = 0;
    
    void parseData() {
        cout << "Parsing data (common logic)" << endl;
    }
    
    virtual void analyzeData() = 0;
    
    void closeFile() {
        cout << "Closing file (common logic)" << endl;
    }
};

class PDFMiner : public DataMiner {
protected:
    void openFile() override {
        cout << "Opening PDF file" << endl;
    }
    
    void extractData() override {
        cout << "Extracting data from PDF" << endl;
    }
    
    void analyzeData() override {
        cout << "Analyzing PDF data" << endl;
    }
};

class CSVMiner : public DataMiner {
protected:
    void openFile() override {
        cout << "Opening CSV file" << endl;
    }
    
    void extractData() override {
        cout << "Extracting data from CSV" << endl;
    }
    
    void analyzeData() override {
        cout << "Analyzing CSV data" << endl;
    }
};

// === 17. STATE PATTERN ===
/*
PURPOSE: Object behavior changes based on state
USE CASE: TCP connections, vending machines
*/

class State {
public:
    virtual void insertCoin() = 0;
    virtual void ejectCoin() = 0;
    virtual void dispense() = 0;
    virtual ~State() {}
};

class VendingMachine;

class NoCoinState : public State {
private:
    VendingMachine* machine;
    
public:
    NoCoinState(VendingMachine* m) : machine(m) {}
    void insertCoin() override;
    void ejectCoin() override {
        cout << "No coin to eject" << endl;
    }
    void dispense() override {
        cout << "Insert coin first" << endl;
    }
};

class HasCoinState : public State {
private:
    VendingMachine* machine;
    
public:
    HasCoinState(VendingMachine* m) : machine(m) {}
    void insertCoin() override {
        cout << "Coin already inserted" << endl;
    }
    void ejectCoin() override;
    void dispense() override;
};

class VendingMachine {
private:
    State* noCoinState;
    State* hasCoinState;
    State* currentState;
    
public:
    VendingMachine() {
        noCoinState = new NoCoinState(this);
        hasCoinState = new HasCoinState(this);
        currentState = noCoinState;
    }
    
    void setState(State* s) { currentState = s; }
    State* getNoCoinState() { return noCoinState; }
    State* getHasCoinState() { return hasCoinState; }
    
    void insertCoin() { currentState->insertCoin(); }
    void ejectCoin() { currentState->ejectCoin(); }
    void dispense() { currentState->dispense(); }
    
    ~VendingMachine() {
        delete noCoinState;
        delete hasCoinState;
    }
};

void NoCoinState::insertCoin() {
    cout << "Coin inserted" << endl;
    machine->setState(machine->getHasCoinState());
}

void HasCoinState::ejectCoin() {
    cout << "Coin ejected" << endl;
    machine->setState(machine->getNoCoinState());
}

void HasCoinState::dispense() {
    cout << "Dispensing product" << endl;
    machine->setState(machine->getNoCoinState());
}

// === 18. ITERATOR PATTERN ===
/*
PURPOSE: Access elements sequentially without exposing structure
USE CASE: Collections, containers
*/

template<typename T>
class Iterator {
public:
    virtual bool hasNext() = 0;
    virtual T next() = 0;
    virtual ~Iterator() {}
};

template<typename T>
class Collection {
public:
    virtual Iterator<T>* createIterator() = 0;
    virtual ~Collection() {}
};

template<typename T>
class ArrayCollection : public Collection<T> {
private:
    vector<T> items;
    
    class ArrayIterator : public Iterator<T> {
    private:
        vector<T>& items;
        int position;
        
    public:
        ArrayIterator(vector<T>& arr) : items(arr), position(0) {}
        
        bool hasNext() override {
            return position < items.size();
        }
        
        T next() override {
            return items[position++];
        }
    };
    
public:
    void add(T item) { items.push_back(item); }
    
    Iterator<T>* createIterator() override {
        return new ArrayIterator(items);
    }
};

// === 19. CHAIN OF RESPONSIBILITY ===
/*
PURPOSE: Pass request along chain until handled
USE CASE: Event handling, logging levels
*/

class Handler {
protected:
    Handler* next;
    
public:
    Handler() : next(nullptr) {}
    
    void setNext(Handler* h) { next = h; }
    
    virtual void handleRequest(int level) = 0;
    virtual ~Handler() {}
};

class InfoHandler : public Handler {
public:
    void handleRequest(int level) override {
        if (level == 1) {
            cout << "Info: Handling request" << endl;
        } else if (next) {
            next->handleRequest(level);
        }
    }
};

class WarningHandler : public Handler {
public:
    void handleRequest(int level) override {
        if (level == 2) {
            cout << "Warning: Handling request" << endl;
        } else if (next) {
            next->handleRequest(level);
        }
    }
};

class ErrorHandler : public Handler {
public:
    void handleRequest(int level) override {
        if (level == 3) {
            cout << "Error: Handling request" << endl;
        } else if (next) {
            next->handleRequest(level);
        }
    }
};

// === 20-23. MORE PATTERNS (Quick Overview) ===

// MEDIATOR: Central communication hub
// MEMENTO: Save/restore object state
// VISITOR: Add operations to objects without changing them
// INTERPRETER: Define grammar and interpret sentences

/*
===============================================================================
PART 5: SOLID PRINCIPLES
===============================================================================
*/

// === S - SINGLE RESPONSIBILITY PRINCIPLE ===
/*
A class should have only ONE reason to change
*/

// BAD: Multiple responsibilities
class BadEmployee {
public:
    void calculateSalary() { /* ... */ }
    void saveToDatabase() { /* ... */ }  // Different responsibility!
    void generateReport() { /* ... */ }  // Different responsibility!
};

// GOOD: Separate responsibilities
class GoodEmployee {
private:
    string name;
    double salary;
    
public:
    double calculateSalary() { return salary; }
    string getName() { return name; }
};

class EmployeeRepository {
public:
    void save(GoodEmployee& emp) { /* Save to DB */ }
    GoodEmployee load(int id) { return GoodEmployee(); }
};

class EmployeeReportGenerator {
public:
    void generateReport(GoodEmployee& emp) { /* Generate report */ }
};

// === O - OPEN/CLOSED PRINCIPLE ===
/*
Open for extension, Closed for modification
*/

// BAD: Need to modify class to add new shapes
class BadAreaCalculator {
public:
    double calculate(string shape, double param1, double param2 = 0) {
        if (shape == "circle") return 3.14 * param1 * param1;
        if (shape == "rectangle") return param1 * param2;
        // Need to modify to add new shapes!
        return 0;
    }
};

// GOOD: Extend through inheritance
class ShapeOCP {
public:
    virtual double area() = 0;
    virtual ~ShapeOCP() {}
};

class CircleOCP : public ShapeOCP {
private:
    double radius;
public:
    CircleOCP(double r) : radius(r) {}
    double area() override { return 3.14 * radius * radius; }
};

class RectangleOCP : public ShapeOCP {
private:
    double length, width;
public:
    RectangleOCP(double l, double w) : length(l), width(w) {}
    double area() override { return length * width; }
};

class AreaCalculatorOCP {
public:
    double calculate(ShapeOCP* shape) {
        return shape->area();  // No modification needed for new shapes!
    }
};

// === L - LISKOV SUBSTITUTION PRINCIPLE ===
/*
Derived classes must be substitutable for base classes
*/

// BAD: Violates LSP
class BirdLSP {
public:
    virtual void fly() { cout << "Flying" << endl; }
};

class PenguinLSP : public BirdLSP {
public:
    void fly() override {
        throw runtime_error("Penguins can't fly!");  // Violates LSP!
    }
};

// GOOD: Proper hierarchy
class BirdGoodLSP {
public:
    virtual void eat() { cout << "Eating" << endl; }
    virtual ~BirdGoodLSP() {}
};

class FlyingBird : public BirdGoodLSP {
public:
    virtual void fly() { cout << "Flying" << endl; }
};

class Sparrow : public FlyingBird {
public:
    void fly() override { cout << "Sparrow flying" << endl; }
};

class PenguinGoodLSP : public BirdGoodLSP {
public:
    void swim() { cout << "Swimming" << endl; }
};

// === I - INTERFACE SEGREGATION PRINCIPLE ===
/*
Clients shouldn't depend on interfaces they don't use
*/

// BAD: Fat interface
class BadWorker {
public:
    virtual void work() = 0;
    virtual void eat() = 0;
    virtual void sleep() = 0;
    virtual ~BadWorker() {}
};

class RobotWorker : public BadWorker {
public:
    void work() override { cout << "Working" << endl; }
    void eat() override { /* Robots don't eat! */ }  // Forced to implement!
    void sleep() override { /* Robots don't sleep! */ }  // Forced to implement!
};

// GOOD: Segregated interfaces
class Workable {
public:
    virtual void work() = 0;
    virtual ~Workable() {}
};

class Eatable {
public:
    virtual void eat() = 0;
    virtual ~Eatable() {}
};

class Sleepable {
public:
    virtual void sleep() = 0;
    virtual ~Sleepable() {}
};

class HumanWorker : public Workable, public Eatable, public Sleepable {
public:
    void work() override { cout << "Human working" << endl; }
    void eat() override { cout << "Human eating" << endl; }
    void sleep() override { cout << "Human sleeping" << endl; }
};

class GoodRobotWorker : public Workable {
public:
    void work() override { cout << "Robot working" << endl; }
    // Only implements what it needs!
};

// === D - DEPENDENCY INVERSION PRINCIPLE ===
/*
Depend on abstractions, not concretions
High-level modules shouldn't depend on low-level modules
*/

// BAD: Direct dependency on concrete class
class MySQLDatabase {
public:
    void connect() { cout << "Connecting to MySQL" << endl; }
    void execute(string query) { cout << "Executing: " << query << endl; }
};

class BadUserService {
private:
    MySQLDatabase db;  // Tightly coupled!
    
public:
    void getUser(int id) {
        db.connect();
        db.execute("SELECT * FROM users WHERE id=" + to_string(id));
    }
};

// GOOD: Depend on abstraction
class Database {
public:
    virtual void connect() = 0;
    virtual void execute(string query) = 0;
    virtual ~Database() {}
};

class MySQLDatabaseDIP : public Database {
public:
    void connect() override { cout << "Connecting to MySQL" << endl; }
    void execute(string query) override { cout << "Executing: " << query << endl; }
};

class PostgreSQLDatabase : public Database {
public:
    void connect() override { cout << "Connecting to PostgreSQL" << endl; }
    void execute(string query) override { cout << "Executing: " << query << endl; }
};

class GoodUserService {
private:
    Database* db;  // Depends on abstraction!
    
public:
    GoodUserService(Database* database) : db(database) {}
    
    void getUser(int id) {
        db->connect();
        db->execute("SELECT * FROM users WHERE id=" + to_string(id));
    }
};

/*
===============================================================================
PART 6: MEMORY MANAGEMENT & SMART POINTERS
===============================================================================
*/

// === RAW POINTERS (Manual Memory Management) ===
void rawPointerExample() {
    int* ptr = new int(42);  // Allocate
    cout << *ptr << endl;
    delete ptr;  // MUST manually delete!
    ptr = nullptr;  // Good practice
}

// === UNIQUE_PTR (Exclusive ownership) ===
void uniquePtrExample() {
    unique_ptr<int> ptr1 = make_unique<int>(42);
    cout << *ptr1 << endl;
    
    // unique_ptr<int> ptr2 = ptr1;  // ERROR: Can't copy!
    unique_ptr<int> ptr2 = move(ptr1);  // OK: Transfer ownership
    // ptr1 is now nullptr
    
    // Automatic cleanup when goes out of scope
}

// === SHARED_PTR (Shared ownership with reference counting) ===
void sharedPtrExample() {
    shared_ptr<int> ptr1 = make_shared<int>(42);
    cout << "Count: " << ptr1.use_count() << endl;  // 1
    
    {
        shared_ptr<int> ptr2 = ptr1;  // Share ownership
        cout << "Count: " << ptr1.use_count() << endl;  // 2
        cout << *ptr2 << endl;
    }  // ptr2 destroyed
    
    cout << "Count: " << ptr1.use_count() << endl;  // 1
    // Memory freed when last shared_ptr is destroyed
}

// === WEAK_PTR (Non-owning reference to avoid circular references) ===
class Node {
public:
    shared_ptr<Node> next;
    weak_ptr<Node> prev;  // Use weak_ptr to break circular reference!
    int data;
    
    Node(int d) : data(d) {
        cout << "Node " << data << " created" << endl;
    }
    
    ~Node() {
        cout << "Node " << data << " destroyed" << endl;
    }
};

void weakPtrExample() {
    shared_ptr<Node> node1 = make_shared<Node>(1);
    shared_ptr<Node> node2 = make_shared<Node>(2);
    
    node1->next = node2;
    node2->prev = node1;  // weak_ptr doesn't increase ref count
    
    // Both nodes will be properly destroyed
}

// === CUSTOM DELETERS ===
void customDeleterExample() {
    auto deleter = [](int* ptr) {
        cout << "Custom delete: " << *ptr << endl;
        delete ptr;
    };
    
    unique_ptr<int, decltype(deleter)> ptr(new int(42), deleter);
}

/*
===============================================================================
PART 7: STL & OOP
===============================================================================
*/

// === FUNCTION OBJECTS (Functors) ===
class Adder {
private:
    int value;
    
public:
    Adder(int v) : value(v) {}
    
    int operator()(int x) {
        return x + value;
    }
};

void functorExample() {
    Adder add5(5);
    cout << add5(10) << endl;  // 15
    
    vector<int> nums = {1, 2, 3, 4, 5};
    transform(nums.begin(), nums.end(), nums.begin(), add5);
    // nums is now {6, 7, 8, 9, 10}
}

// === COMPARATORS FOR SORTING ===
class Student2 {
public:
    string name;
    int marks;
    
    Student2(string n, int m) : name(n), marks(m) {}
};

class CompareByMarks {
public:
    bool operator()(const Student2& a, const Student2& b) {
        return a.marks > b.marks;  // Descending order
    }
};

void comparatorExample() {
    vector<Student2> students = {
        Student2("Alice", 85),
        Student2("Bob", 92),
        Student2("Charlie", 78)
    };
    
    sort(students.begin(), students.end(), CompareByMarks());
}

// === LAMBDA EXPRESSIONS (Modern C++) ===
void lambdaExample() {
    vector<int> nums = {1, 2, 3, 4, 5};
    
    // Lambda with capture
    int multiplier = 3;
    for_each(nums.begin(), nums.end(), [multiplier](int& n) {
        n *= multiplier;
    });
    
    // Lambda as comparator
    sort(nums.begin(), nums.end(), [](int a, int b) {
        return a > b;  // Descending
    });
}

/*
===============================================================================
PART 8: INTERVIEW QUESTIONS & BEST PRACTICES
===============================================================================
*/

// === COMMON INTERVIEW QUESTIONS ===

/*
Q1: What is the difference between class and struct in C++?
A: Default access is private for class, public for struct.
   Both can have methods, inheritance, etc.

Q2: What is virtual function?
A: Function that can be overridden in derived class.
   Enables runtime polymorphism.

Q3: What is pure virtual function?
A: Virtual function with = 0, makes class abstract.
   Must be implemented by derived classes.

Q4: What is virtual destructor and why needed?
A: Destructor declared virtual ensures proper cleanup when
   deleting derived class object through base class pointer.

Q5: What is diamond problem?
A: Ambiguity in multiple inheritance when two parent classes
   inherit from same grandparent. Solved with virtual inheritance.

Q6: What is difference between shallow and deep copy?
A: Shallow: Copies pointer, both point to same memory.
   Deep: Creates new memory, independent copies.

Q7: What is difference between overloading and overriding?
A: Overloading: Same name, different parameters (compile-time).
   Overriding: Same signature in derived class (runtime).

Q8: What is static polymorphism vs dynamic polymorphism?
A: Static: Compile-time (function/operator overloading).
   Dynamic: Runtime (virtual functions).

Q9: What is RAII?
A: Resource Acquisition Is Initialization.
   Resources acquired in constructor, released in destructor.

Q10: What is rule of three/five?
A: If class needs custom destructor, copy constructor, or
    copy assignment, it likely needs all three (rule of 3).
    Add move constructor and move assignment (rule of 5).
*/

// === RULE OF THREE/FIVE EXAMPLE ===
class RuleOfFive {
private:
    int* data;
    size_t size;
    
public:
    // Constructor
    RuleOfFive(size_t s) : size(s) {
        data = new int[size];
    }
    
    // 1. Destructor
    ~RuleOfFive() {
        delete[] data;
    }
    
    // 2. Copy Constructor (Deep copy)
    RuleOfFive(const RuleOfFive& other) : size(other.size) {
        data = new int[size];
        copy(other.data, other.data + size, data);
    }
    
    // 3. Copy Assignment
    RuleOfFive& operator=(const RuleOfFive& other) {
        if (this != &other) {
            delete[] data;
            size = other.size;
            data = new int[size];
            copy(other.data, other.data + size, data);
        }
        return *this;
    }
    
    // 4. Move Constructor
    RuleOfFive(RuleOfFive&& other) noexcept 
        : data(other.data), size(other.size) {
        other.data = nullptr;
        other.size = 0;
    }
    
    // 5. Move Assignment
    RuleOfFive& operator=(RuleOfFive&& other) noexcept {
        if (this != &other) {
            delete[] data;
            data = other.data;
            size = other.size;
            other.data = nullptr;
            other.size = 0;
        }
        return *this;
    }
};

// === COPY CONSTRUCTOR DEEP DIVE ===
class DeepCopyExample {
private:
    int* ptr;
    
public:
    DeepCopyExample(int val) {
        ptr = new int(val);
    }
    
    // Deep copy constructor
    DeepCopyExample(const DeepCopyExample& obj) {
        ptr = new int(*obj.ptr);  // Create new memory
    }
    
    ~DeepCopyExample() {
        delete ptr;
    }
    
    void setValue(int val) { *ptr = val; }
    int getValue() { return *ptr; }
};

// === DIAMOND PROBLEM & SOLUTION ===
class GrandParent {
public:
    int value;
    GrandParent() : value(0) {}
};

// Virtual inheritance solves diamond problem
class Parent1 : virtual public GrandParent {
public:
    Parent1() {}
};

class Parent2 : virtual public GrandParent {
public:
    Parent2() {}
};

class Child : public Parent1, public Parent2 {
public:
    Child() {}
    // Now only ONE copy of GrandParent's value exists
};

// === BEST PRACTICES CHECKLIST ===

/*
✅ ENCAPSULATION:
- Keep data members private
- Provide public getters/setters with validation
- Hide implementation details

✅ INHERITANCE:
- Use "IS-A" relationship only
- Prefer composition over inheritance when possible
- Make base class destructors virtual
- Use protected for members accessed by derived classes

✅ POLYMORPHISM:
- Use virtual functions for runtime polymorphism
- Mark overrides with 'override' keyword
- Use pure virtual for interfaces
- Return references/pointers to base class for flexibility

✅ MEMORY MANAGEMENT:
- Use smart pointers (unique_ptr, shared_ptr)
- Follow RAII principle
- Implement rule of three/five when needed
- Avoid memory leaks with proper cleanup

✅ DESIGN:
- Single Responsibility Principle
- Keep classes small and focused
- Use design patterns appropriately
- Write self-documenting code
- Prefer const correctness

✅ PERFORMANCE:
- Pass large objects by const reference
- Use move semantics for efficiency
- Return by value for small objects (RVO)
- Avoid unnecessary copies

✅ MODERN C++:
- Use auto keyword appropriately
- Use range-based for loops
- Use nullptr instead of NULL
- Use enum class instead of enum
- Use constexpr for compile-time constants
*/

// === CODING INTERVIEW PATTERNS ===

// 1. Implement LRU Cache using OOP
class LRUCache {
private:
    struct Node {
        int key, value;
        Node *prev, *next;
        Node(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr) {}
    };
    
    int capacity;
    unordered_map<int, Node*> cache;
    Node *head, *tail;
    
    void addNode(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }
    
    void removeNode(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }
    
    void moveToHead(Node* node) {
        removeNode(node);
        addNode(node);
    }
    
    Node* popTail() {
        Node* node = tail->prev;
        removeNode(node);
        return node;
    }
    
public:
    LRUCache(int cap) : capacity(cap) {
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if (cache.find(key) == cache.end()) return -1;
        Node* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void put(int key, int value) {
        if (cache.find(key) != cache.end()) {
            Node* node = cache[key];
            node->value = value;
            moveToHead(node);
        } else {
            Node* node = new Node(key, value);
            cache[key] = node;
            addNode(node);
            
            if (cache.size() > capacity) {
                Node* tail_node = popTail();
                cache.erase(tail_node->key);
                delete tail_node;
            }
        }
    }
    
    ~LRUCache() {
        for (auto& pair : cache) delete pair.second;
        delete head;
        delete tail;
    }
};

// 2. Design Parking Lot System
enum VehicleType { CAR, TRUCK, MOTORCYCLE };

class Vehicle {
protected:
    string licensePlate;
    VehicleType type;
    
public:
    Vehicle(string plate, VehicleType t) : licensePlate(plate), type(t) {}
    VehicleType getType() { return type; }
    string getPlate() { return licensePlate; }
};

class ParkingSpot {
private:
    int spotNumber;
    VehicleType type;
    Vehicle* parkedVehicle;
    
public:
    ParkingSpot(int num, VehicleType t) 
        : spotNumber(num), type(t), parkedVehicle(nullptr) {}
    
    bool isAvailable() { return parkedVehicle == nullptr; }
    
    bool park(Vehicle* vehicle) {
        if (!isAvailable() || vehicle->getType() != type) return false;
        parkedVehicle = vehicle;
        return true;
    }
    
    void removeVehicle() { parkedVehicle = nullptr; }
};

class ParkingLot {
private:
    vector<ParkingSpot*> spots;
    
public:
    void addSpot(ParkingSpot* spot) {
        spots.push_back(spot);
    }
    
    bool parkVehicle(Vehicle* vehicle) {
        for (auto spot : spots) {
            if (spot->park(vehicle)) return true;
        }
        return false;
    }
    
    int getAvailableSpots(VehicleType type) {
        int count = 0;
        for (auto spot : spots) {
            if (spot->isAvailable()) count++;
        }
        return count;
    }
};

/*
===============================================================================
LEARNING ROADMAP (0 to 100)
===============================================================================

WEEK 1-2: FUNDAMENTALS
□ Classes and Objects
□ Constructors and Destructors
□ Access Modifiers
□ This Pointer
□ Static Members
□ Friend Functions

WEEK 3: INHERITANCE
□ Single Inheritance
□ Multiple Inheritance
□ Multilevel Inheritance
□ Virtual Base Classes
□ Access Control in Inheritance

WEEK 4: POLYMORPHISM
□ Function Overloading
□ Operator Overloading
□ Virtual Functions
□ Pure Virtual Functions
□ Abstract Classes
□ Runtime Polymorphism

WEEK 5: ADVANCED CONCEPTS
□ Const Correctness
□ Composition vs Aggregation
□ Copy Constructor (Deep/Shallow)
□ Move Semantics
□ Rule of Three/Five

WEEK 6: MEMORY MANAGEMENT
□ Raw Pointers
□ Smart Pointers (unique, shared, weak)
□ RAII Principle
□ Memory Leaks Prevention

WEEK 7-8: DESIGN PATTERNS
□ Creational Patterns (5)
□ Structural Patterns (7)
□ Behavioral Patterns (11)

WEEK 9: SOLID PRINCIPLES
□ Single Responsibility
□ Open/Closed
□ Liskov Substitution
□ Interface Segregation
□ Dependency Inversion

WEEK 10: PRACTICE & PROJECTS
□ Design LRU Cache
□ Design Parking Lot
□ Design Library Management System
□ Design ATM System
□ Design Social Media App

===============================================================================
MUST-KNOW CONCEPTS FOR INTERVIEWS
===============================================================================

CORE CONCEPTS (Must Know 100%):
✓ Four Pillars of OOP
✓ Virtual Functions & Polymorphism
✓ Constructor/Destructor
✓ Access Modifiers
✓ Inheritance Types
✓ Abstract Classes & Interfaces
✓ Copy Constructor & Assignment
✓ Static Members

ADVANCED (80% Coverage):
✓ Smart Pointers
✓ Move Semantics
✓ Rule of Three/Five
✓ SOLID Principles
✓ Common Design Patterns (at least 10)
✓ Virtual Destructor
✓ Diamond Problem
✓ RAII

SYSTEM DESIGN (OOP Focus):
✓ Class Diagram Design
✓ Relationship Mapping (IS-A, HAS-A)
✓ Design Patterns Application
✓ Low-Level Design Problems

===============================================================================
PRACTICE PROBLEMS
===============================================================================

EASY:
1. Create a Student Management System
2. Implement a Simple Calculator with operator overloading
3. Design a Shape hierarchy with area calculation
4. Create a Bank Account system with inheritance
5. Implement a Library Book class

MEDIUM:
1. Design a Parking Lot System
2. Implement LRU Cache
3. Design a Vending Machine
4. Create an Online Shopping Cart
5. Design a Hotel Reservation System
6. Implement a File System (folders and files)
7. Design a Deck of Cards
8. Create a Snake and Ladder Game

HARD:
1. Design a Social Media Platform (Users, Posts, Comments)
2. Implement an ATM System
3. Design an Elevator System
4. Create a Chess Game
5. Design a Movie Ticket Booking System
6. Implement a Restaurant Management System
7. Design a Ride-Sharing Application
8. Create a Hospital Management System

===============================================================================
FINAL CHECKLIST - YOU'RE READY WHEN...
===============================================================================

□ Can explain all four pillars with examples
□ Can write virtual functions and explain vtable
□ Understand when to use each type of inheritance
□ Can implement deep copy correctly
□ Know when to use smart pointers vs raw pointers
□ Can apply at least 10 design patterns
□ Understand and apply SOLID principles
□ Can design class hierarchies for real-world problems
□ Know difference between compile-time and runtime polymorphism
□ Can explain memory management and avoid leaks
□ Understand const correctness
□ Can implement rule of three/five
□ Have solved 20+ OOP design problems

===============================================================================
QUICK REFERENCE CARD
===============================================================================

CLASS SYNTAX:
class Name {
private:      // Default access
    int data;
protected:
    void method();
public:
    Name() { }  // Constructor
    ~Name() { } // Destructor
};

INHERITANCE:
class Derived : public Base { };

VIRTUAL FUNCTION:
virtual void func() = 0;  // Pure virtual (abstract)
virtual void func() { }   // Virtual

POLYMORPHISM:
Base* ptr = new Derived();
ptr->virtualFunc();  // Calls Derived's version

SMART POINTERS:
unique_ptr<T> ptr = make_unique<T>();
shared_ptr<T> ptr = make_shared<T>();
weak_ptr<T> ptr = sharedPtr;

OPERATOR OVERLOADING:
ReturnType operator+(const Type& obj) { }

===============================================================================
CONGRATULATIONS! 🎉
You now have complete mastery over Object-Oriented Programming in C++!
Practice consistently and you'll excel in interviews and real-world projects.
===============================================================================
*/

// Main function demonstrating various concepts
int main() {
    cout << "OOP Concepts Demonstration" << endl;
    
    // Uncomment to run specific examples:
    // polymorphismDemo();
    // virtualDestructorDemo();
    // decoratorDemo();
    // staticExample();
    
    return 0;
}