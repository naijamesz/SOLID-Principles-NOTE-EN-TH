# S.O.L.I.D. Principles: 5 Notes

## What are SOLID Design Principles?

S.O.L.I.D. คือหลักการเขียนโปรแกรมที่ช่วยให้โค้ดมีความยืดหยุ่น ง่ายต่อการบำรุงรักษา ลดการซ้ำซ้อน และเพิ่มความปลอดภัย โดยสามารถแก้ไขหรือเพิ่มฟีเจอร์ได้โดยไม่ส่งผลกระทบต่อโมดูลอื่น ๆ อย่างมาก

---

## SRP - Single Responsibility Principle

- หลักการที่ว่าคลาสควรมีความรับผิดชอบเพียงอย่างเดียว

##### Bad Code

```typescript
class UserService {
  createUser(name: string) {
    console.log(`User ${name} created`);
  }

  sendWelcomeEmail(name: string) {
    console.log(`Sending welcome email to ${name}`);
  }
}
```

**ปัญหา**: คลาส `UserService` มีหน้าที่หลายอย่าง ทั้งการจัดการผู้ใช้และการส่งอีเมล ทำให้โค้ดซับซ้อนและยากต่อการแก้ไขเมื่อมีการเปลี่ยนแปลง

##### Best Practices

```typescript
class UserService {
  createUser(name: string) {
    console.log(`User ${name} created`);
  }
}

class EmailService {
  sendWelcomeEmail(name: string) {
    console.log(`Sending welcome email to ${name}`);
  }
}
```

**ปรับปรุง**: แยกหน้าที่ออกจากกัน คลาส `UserService` จัดการผู้ใช้ ส่วน `EmailService` รับผิดชอบการจัดการอีเมล

**Use case**: การแยกการทำงานช่วยให้สามารถปรับปรุงแต่ละส่วนได้ง่าย เช่น หากต้องการเปลี่ยนวิธีส่งอีเมลก็สามารถทำได้โดยไม่กระทบกับการจัดการผู้ใช้

---

## OCP - Open-Closed Principle

- คลาสควรเปิดให้ขยาย (extend) แต่ปิดไม่ให้แก้ไขโดยตรง

##### Bad Code

```typescript
class Shape {
  type: string;

  constructor(type: string) {
    this.type = type;
  }

  getArea() {
    if (this.type === 'circle') {
      return Math.PI * 10 * 10;
    } else if (this.type === 'square') {
      return 20 * 20;
    }
  }
}
```

**ปัญหา**: เมื่อเพิ่มประเภทใหม่ เช่น รูปสามเหลี่ยม ต้องแก้ไขโค้ดภายในคลาส `Shape` ซึ่งทำให้โค้ดมีความซับซ้อน

##### Best Practices

```typescript
abstract class Shape {
  abstract getArea(): number;
}

class Circle extends Shape {
  getArea() {
    return Math.PI * 10 * 10;
  }
}

class Square extends Shape {
  getArea() {
    return 20 * 20;
  }
}

class Triangle extends Shape {
  getArea() {
    return (15 * 10) / 2;
  }
}
```

**ปรับปรุง**: ใช้ `polymorphism` โดยสร้างคลาสย่อยสำหรับแต่ละรูปทรง ทำให้สามารถขยายคลาสได้โดยไม่ต้องแก้ไขโค้ดหลัก

**Use case**: ใช้เมื่อต้องการเพิ่มฟีเจอร์ใหม่โดยไม่ต้องแก้ไขโค้ดเดิม เช่น การเพิ่มรูปแบบการชำระเงินใหม่ในระบบ

---

## LSP - Liskov Substitution Principle

- คลาสย่อยสามารถแทนคลาสหลักได้โดยไม่กระทบการทำงานของระบบ

##### Bad Code

```typescript
class Bird {
  fly() {
    console.log('Flying');
  }
}

class Penguin extends Bird {
  fly() {
    throw new Error("Penguins can't fly");
  }
}
```

**ปัญหา**: `Penguin` ไม่สามารถแทนที่ `Bird` ได้ตามที่คาดหวัง เพราะมันบินไม่ได้

##### Best Practices

```typescript
class Bird {
  move() {
    console.log('Moving');
  }
}

class Penguin extends Bird {
  move() {
    console.log("Penguins can't fly, but they swim.");
  }
}

class Eagle extends Bird {
  move() {
    console.log('Eagle is flying');
  }
}
```

**ปรับปรุง**: เปลี่ยน method `fly` เป็น `move` เพื่อรองรับการเคลื่อนที่ของนกแต่ละชนิด ทำให้ `Penguin` ไม่จำเป็นต้องบิน แต่ยังคงเคลื่อนที่ได้

**Use case**: ใช้เมื่อมีการสืบทอดจากคลาสหลัก และต้องการคงพฤติกรรมที่แตกต่างกันโดยไม่กระทบต่อคลาสหลัก

---

## ISP - Interface Segregation Principle

- ไม่ควรบังคับให้คลาสต้องใช้ interface ที่มี method ที่ไม่จำเป็นต้องใช้

##### Bad Code

```typescript
interface Worker {
  work(): void;
  eat(): void;
}

class Robot implements Worker {
  work() {
    console.log('Robot is working');
  }

  eat() {
    throw new Error("Robot can't eat");
  }
}
```

**ปัญหา**: `Robot` ต้องใช้ method `eat()` ทั้งที่ไม่จำเป็น เพราะมันไม่สามารถกินได้

##### Best Practices

```typescript
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

class Human implements Workable, Eatable {
  work() {
    console.log('Human is working');
  }

  eat() {
    console.log('Human is eating');
  }
}

class Robot implements Workable {
  work() {
    console.log('Robot is working');
  }
}
```

**ปรับปรุง**: แยก interface เป็น `Workable` และ `Eatable` เพื่อให้ `Robot` ไม่ต้อง implement method ที่ไม่จำเป็น

**Use case**: ใช้ในระบบที่มีการทำงานแตกต่างกัน เช่น การจัดการพนักงานกับหุ่นยนต์ในโรงงาน

---

## DIP - Dependency Inversion Principle

- คลาสควรพึ่งพา abstractions (interfaces) มากกว่าการพึ่งพา concretions (implementation)

##### Bad Code

```typescript
class Database {
  connect() {
    console.log('Connecting to database');
  }
}

class UserService {
  private db: Database = new Database();

  getUser() {
    this.db.connect();
    console.log('Getting user data');
  }
}
```

**ปัญหา**: `UserService` พึ่งพา `Database` โดยตรง ทำให้ยากต่อการเปลี่ยนแปลงหรือขยายในอนาคต

##### Best Practices

```typescript
interface IDatabase {
  connect(): void;
}

class Database implements IDatabase {
  connect() {
    console.log('Connecting to database');
  }
}

class UserService {
  private db: IDatabase;

  constructor(db: IDatabase) {
    this.db = db;
  }

  getUser() {
    this.db.connect();
    console.log('Getting user data');
  }
}
```

**ปรับปรุง**: ใช้ interface `IDatabase` เพื่อแยกการเชื่อมต่อฐานข้อมูล ทำให้สามารถเปลี่ยนฐานข้อมูลได้ง่ายในอนาคต

**Use case**: ใช้ในการทำ Dependency Injection (DI) เพื่อเพิ่มความยืดหยุ่นในการจัดการระบบ

---
