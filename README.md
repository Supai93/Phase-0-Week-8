# Phase-0-Week-8

# JavaScript - Week 8: OOP dalam JavaScript

## 1. Perkenalan
JavaScript mendukung pemrograman berbasis objek (OOP) dalam bentuk `prototype`, `class`, dan constructors. Paradigma pemrograman OOP bertumpu pada objek yang dapat menyimpan data dan metode. Ada 4 prinsip utama dalam OOP:
1. **Encapsulation** – Membungkus data dan metode yang mengolah data tersebut dalam satu kesatuan unit.
2. **Abstraction** – Menyembunyikan detil implementasi dan hanya menampakkan fungsi utama.
3. **Inheritance** – Memperoleh properti dan perilaku dari `class` lain.
4. **Polymorphism** – Menyediakan antarmuka umum untuk berbagai bentuk (tipe data).

## 2. Kilas Balik `Prototype`
`Prototype` adalah tulang punggung dari OOP dalam JavaScript yang memungkinkan objek mewarisi properti dan metode dari objek lain. Mari kita kilas balik
```javascript
// Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Adding a method to the prototype
Person.prototype.greet = function() {
  return `Hello, my name is ${this.name} and I'm ${this.age} years old.`;
};

// Creating instances
const john = new Person('John', 30);
console.log(john.greet()); // "Hello, my name is John and I'm 30 years old."
```

## 3. ES6 Classes

ES6 menghadirkan sintaks `class` yang memudahkan kita menulis objek dan melakukan inheritance terutama bagi developer yang berpengalaman dengan bahas pemrograman berbasis `class`. Dibalik layar `class` juga menggunakan konsep prototype.

### Class Declaration

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

const animal = new Animal("Generic Animal");
animal.speak(); // Generic Animal makes a sound.
```

### Metode dan Properti `Static`

Kata kunci `static` memungkinkan properti atau metode dapat diakses tanpa perlu menciptakan suatu objek. Properti atau metode tersebut menjadi milik `class` itu sendiri:

```javascript
class MathOperations {
  static add(x, y) {
    return x + y;
  }
  
  static PI = 3.14159;
}

console.log(MathOperations.add(5, 3)); // 8
console.log(MathOperations.PI); // 3.14159

// This would result in an error:
// const math = new MathOperations();
// math.add(1, 2); // TypeError: math.add is not a function
```

### Getters dan Setters

`Class` dapat memiliki metode `getter` (mengambil) dan `setter` (mengisi) untuk mengontrol akses properti:

```javascript
class Circle {
  constructor(radius) {
    this._radius = radius; // Convention: underscore indicates "private"
  }
  
  get radius() {
    return this._radius;
  }
  set radius(value) {
    if (value <= 0) {
      throw new Error('Radius must be positive');
    }
    this._radius = value;
  }
  
  get area() {
    return Math.PI * this._radius * this._radius;
  }
}

const circle = new Circle(5);
console.log(circle.radius); // 5
console.log(circle.area); // 78.53981633974483

circle.radius = 10;
console.log(circle.radius); // 10

// This would throw an error:
// circle.radius = -5; // Error: Radius must be positive
```

## 4. Encapsulation dalam JavaScript

Encapsulation adalah konsep mengelompokkan data-data dan metode dalam satu kesatuan unit, dan membatasi sebagian akses ke sebagian objek itu. Dahulu, JavaScript sangat terbatas untuk mencapai encapsulation, tetapi fitur baru sudah memungkinkan tercapainya encapsulation.

### Private Fields (ES2022)

JavaScript modern meenghadirkan private class fields menggunakan awalan `#`:
```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const acc = new BankAccount();
acc.deposit(100);
console.log(acc.getBalance()); // 100
```

## 5. Inheritance

Inheritance memungkinkan `class` mewarisi properti dan metode dari `class ` lain.

### Class Inheritance dengan `extends`

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} makes a noise.`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call the parent constructor
    this.breed = breed;
  }
  
  speak() {
    return `${this.name} barks.`;
  }
  
  fetch() {
    return `${this.name} fetches the ball.`;
  }
}

const animal = new Animal('Generic Animal');
console.log(animal.speak()); // "Generic Animal makes a noise."

const dog = new Dog('Rex', 'German Shepherd');
console.log(dog.speak()); // "Rex barks."
console.log(dog.fetch()); // "Rex fetches the ball."
console.log(dog.breed); // "German Shepherd"
```

### Kata kunci `super`

`super` digunakan untuk memanggil metode dari parent si objek:

```javascript
class Cat extends Animal {
  speak() {
    return `${this.name} meows. ${super.speak()}`;
  }
}

const cat = new Cat('Whiskers');
console.log(cat.speak()); // "Whiskers meows. Whiskers makes a noise."
```

### Memeriksa Inheritance

Ada beberapa cara memeriksa relasi inheritance:

```javascript
const dog = new Dog('Rex', 'German Shepherd');

// instanceof checks the prototype chain
console.log(dog instanceof Dog); // true
console.log(dog instanceof Animal); // true
console.log(dog instanceof Object); // true

// isPrototypeOf checks if an object exists in another object's prototype chain
console.log(Animal.prototype.isPrototypeOf(dog)); // true
console.log(Dog.prototype.isPrototypeOf(dog)); // true
```

## 6. Polymorphism

Polymorphism berarti "banyak rupa" yang memungkinkan objek dari class lain dianggap sebagai sebuah superclass umum. Dlaam JavaScript hal ini dicapai lewat overriding metode dan dynamic typing.

### Method Overriding

Seperti terlihat di contoh `Dog` dan `Cat` sebelumnya, keduanya melakukan override pada metode `speak()` yang dimiliki class `Animal` yaitu parent-nya.speak() method from Animal.

## Object Composition

Sebuah komposisi seringkali lebih diutamakan daripada inheritance untuk coding yang flexible dan mudah didaur ulang. Dalam JavaScript kita dapat menggabung-gabugnkan komponen:

### Object.assign()

```javascript
const canEat = {
  eat(food) {
    return `Eating ${food}`;
  }
};

const canSleep = {
  sleep(hours) {
    return `Sleeping for ${hours} hours`;
  }
};

const canFly = {
  fly(altitude) {
    return `Flying at ${altitude} feet`;
  }
};

// Creating a bird using composition
function createBird(name) {
  return Object.assign({}, canEat, canSleep, canFly, { name });
}

const eagle = createBird('Eagle');
console.log(eagle.eat('fish')); // "Eating fish"
console.log(eagle.fly(5000)); // "Flying at 5000 feet"
```

## 7. Tugas-Tugas

### Tugas 1: Buatlah class `Vehicle`
- Tambahkan properti `make`, `model`.
- Tambahkan metode `displayInfo()`.

### Tugas 2: Extend `Vehicle` menjadi `Car`
- Tambahkan properti `numberOfDoors` property.
- Override `displayInfo()` untuk menampilkan info jumlah pintu.

### Tugas 3: Tambahkan private field pada `Car`
- Tambahkan private field `mileage`.
- Tambahkan metode untuk mengubah dan membaca mileage.

### Tugas 4: Hirarki Bangun Datar (Shape)

Buatlah sebuah hirarki bangun datar: sebuah base class `Shape`d dengan metode `area()` dan `perimeter()`, dan turunan class untuk lingkaran (`Circle`), persegi (`Rectangle`), dan segitiga (`Triangle`). Setiap bentuk harus mengimplementasikan metode-metode tersebut.

<details>
<summary>Solution</summary>

```javascript
class Shape {
  constructor() {
    if (this.constructor === Shape) {
      throw new Error('Abstract classes cannot be instantiated.');
    }
  }
  
  area() {
    throw new Error('Method must be implemented by subclass.');
  }
  
  perimeter() {
    throw new Error('Method must be implemented by subclass.');
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  
  area() {
    return Math.PI * this.radius * this.radius;
  }
  
  perimeter() {
    return 2 * Math.PI * this.radius;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }
  
  area() {
    return this.width * this.height;
  }
  
  perimeter() {
    return 2 * (this.width + this.height);
  }
}

class Triangle extends Shape {
  constructor(a, b, c) {
    super();
    this.a = a;
    this.b = b;
    this.c = c;
  }
  
  area() {
    // Heron's formula
    const s = (this.a + this.b + this.c) / 2;
    return Math.sqrt(s * (s - this.a) * (s - this.b) * (s - this.c));
  }
  
  perimeter() {
    return this.a + this.b + this.c;
  }
}

// Testing
const circle = new Circle(5);
console.log(`Circle area: ${circle.area()}`);
console.log(`Circle perimeter: ${circle.perimeter()}`);

const rectangle = new Rectangle(4, 6);
console.log(`Rectangle area: ${rectangle.area()}`);
console.log(`Rectangle perimeter: ${rectangle.perimeter()}`);

const triangle = new Triangle(3, 4, 5);
console.log(`Triangle area: ${triangle.area()}`);
console.log(`Triangle perimeter: ${triangle.perimeter()}`);
```
</details>

## Tautan
- MDN Web Docs: [JavaScript Object-Oriented Programming](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_programming)
- JavaScript.info: [Classes](https://javascript.info/classes)
