---
icon: hand-wave
description: ประกฤษฎิ์ อรุณกิจเจริญ 650710563
---

# Method Hiding

<mark style="color:blue;">Method Hiding</mark> หมายถึง หมายถึงการซ่อนหรือปกปิด เมธอดของคลาสพ่อแม่ (base class) โดยที่คลาสลูก (derived class) สร้างเมธอดที่มีชื่อเหมือนกันขึ้นมา

แต่ไม่ได้ใช้การสืบทอดผ่านการ <mark style="color:orange;">**override**</mark> แต่เป็นการสร้างเมธอดใหม่ ซึ่งเมธอดในคลาสลูกจะ "ซ่อน" เมธอดของคลาสพ่อแม่เมื่อมีการอ้างอิงหรือสร้างobjectเป็นประเภทของคลาสลูก

{% hint style="info" %}
มีลักษณะการทำงานที่ต่างออกไปในบริบทของ <mark style="color:yellow;">**static methods**</mark> หรือ <mark style="color:green;">**non-static methods**</mark> และอาจเกิดขึ้นได้โดยอัตโนมัติหากเรามีการนิยามเมธอดที่ซ้ำชื่อกัน
{% endhint %}

<mark style="color:red;">**ในภาษา C#**</mark> เมื่อสร้างเมธอดในคลาสลูกที่มีsignature เหมือนกับเมธอดในคลาสพ่อแม่ แต่ใช้คำสั่ง `new` เพื่อบอกคอมไพเลอร์ว่าเมธอดนี้ไม่ใช่การสืบทอดแบบ overriding แต่เป็นการซ่อนเมธอดแทน

> ตัวอย่างนี้เป็น Method Hiding แบบ non-static method

{% tabs %}
{% tab title="C#" %}
```csharp
class Parent
{
    public void Print()
    {
        Console.WriteLine("Parent class Print method");
    }

    public void Display()
    {
        Console.WriteLine("Parent class Display method");
    }
}

class Child : Parent
{
    public new void Print()
    {
        Console.WriteLine("Child class Print method");
    }
}

class Program
{
    static void Main()
    {
        Parent p = new Parent();
        Child c = new Child();
        Parent pRefToC = new Child();

        p.Print();           
        p.Display();         
        c.Print();           
        c.Display();         
        pRefToC.Print();   //เป็นการอ้างอิง พ่อแม่ แต่ชึ้ไปที่ลูก  
    }
}
console.log(message);
```
{% endtab %}

{% tab title="Output" %}
```
Parent class Print method
Parent class Display method
Child class Print method
Parent class Display method 
Parent class Print method
```
{% endtab %}
{% endtabs %}

จะเห็นว่าในคลาส `Parent` มีเมธอด `Print()` และ `Display()` ในคลาส `Child` มีเมธอด `Print()` ถูกซ่อนด้วยkeyword`new`, แต่เมธอด `Display()` ไม่ได้ถูกซ่อน

&#x20; ผลลัพธ์เมื่อเรียก `Print()` ผ่านออบเจกต์ของ `Child` จะเป็นเมธอดจาก `Child`เมื่อเรียก `Display()` ผ่านObjectของ `Child`, เมธอดจาก `Parent` จะถูกเรียกใช้เพราะไม่ได้ซ่อน

{% hint style="info" %}
จะเห็นว่า การใช้ new ในการ <mark style="color:blue;">Method Hiding</mark> ไม่ได้มีผลลัพธ์ต่างจากการไม่ใช้ ใน C# keyword new ช่วยแค่การซ่อนตัวพ่อแม่ ทำให้ Code อ่านง่ายขึ้น และช่วยป้องกันเตือน ไม่ให้ไปยุ่งกับพ่อแม่
{% endhint %}

> ตัวอย่างนี้เป็น Method Hiding แบบ static method

{% tabs %}
{% tab title="C#" %}
```csharp
using System;

class Animal
{
    public static void Speak()
    {
        Console.WriteLine("Animal static speaks");
    }
}

class Dog : Animal
{
    public new static void Speak()  // ซ่อน static method ของ Animal
    {
        Console.WriteLine("Dog static barks");
    }
}

class Program
{
    static void Main()
    {
        Animal.Speak();            
        Dog.Speak();               

        Animal animalRefToDog = new Dog();
        animalRefToDog.Speak();   
    }
}
```
{% endtab %}

{% tab title="Output" %}
```
Animal static speaks
Dog static barks
Animal static speaks  //(method hiding, เพราะอ้างอิงจากตัวแปร)
```
{% endtab %}
{% endtabs %}

มีผลลัพธ์ที่ไม่ต่างกัน ต่างกันแค่การอ้างอิง static method ถูกเรียกใช้จากตัวคลาสโดยตรง (ไม่ต้องสร้างออบเจ็กต์) การเลือกเมธอดจะทำตามประเภทของตัวแปรอ้างอิง (reference type) เสมอ ไม่มีการทำงานแบบ **dynamic dispatch**

## Java

ในภาษา Java นั้น <mark style="color:blue;">"Method Hiding"</mark> เป็นแนวคิดที่คล้ายกับ <mark style="color:orange;">Method Overriding</mark> แต่จะเกี่ยวข้องกับการซ่อนหรือซับซ้อนการทำงานของ method ที่เป็น static ของ superclass โดยการประกาศ method ที่มีลักษณะเหมือนกันใน subclass ซึ่งถ้าเป็นกรณีที่ method เป็น static มันจะทำการ "hide" method ของ superclass แทนที่จะ override เหมือน method ปกติ

แต่อ่านแบบนี้แล้วงง ไปดูตัวอย่างกันดีกว่า

> แบบ non-static method

{% tabs %}
{% tab title="Java" %}
```java
class Animal {
    public void move() {
        System.out.println("Animal is moving");
    }
}

class Dog extends Animal {
    @Override
    public void move() {
        System.out.println("Dog is running");
    }
}

public class TestMethodHiding {
    public static void main(String[] args) {
        Animal animal = new Animal();
        Animal dogAsAnimal = new Dog();
        Dog dog = new Dog();

        animal.move();        
        dogAsAnimal.move();    
        dog.move();            
}
```
{% endtab %}

{% tab title="Output" %}
```
Animal is moving
Dog is running //(Overriding occurs here)
Dog is running
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
ตัวอย่างด้านบนคือ Method overriding ทำให้ method ของ คลาสลูก ถูกเรียกใช้แทนที่ method ของ คลาสแม่ ตามชนิดของ object ขณะที่ **Method Hiding** ขึ้นอยู่กับชนิดของ reference
{% endhint %}

Method hiding จะเกิดเมื่อ มีการใช้ static method

> แบบ static method

{% tabs %}
{% tab title="Java" %}
```java
class Mam {
    public static void sound() {
        System.out.println("Mam make a sound");
    }
}

class Son extends Mam {
    public static void sound() {
        System.out.println("Son make a sound");
    }
}

public class TT {
    public static void main(String[] args) {
        Mam m = new Mam();
        Mam sonAsMam = new Son();
        Son s = new Son();

        m.sound();        
        sonAsMam.sound();   
        s.sound();           
    }
}
```
{% endtab %}

{% tab title="Output" %}
```
Mam make a sound
Mam make a sound //(Method Hiding occurs here)
Son make a sound 
```
{% endtab %}
{% endtabs %}

เมื่อ method เป็น static จะไม่สามารถ override ได้เหมือนกับ method ปกติ แต่สามารถ **Method Hiding** ในกรณีนี้ method ของ ลูก(`Son`) จะซ่อน method ของ แม่ (`Mam`) ไว้

## C ในภาษา C ไม่มี Method hiding !!!

## Python

ใน <mark style="color:red;">Python ไม่มีการ Method hiding</mark> แต่มีการ Data hiding คือ การซ่อนข้อมูล (attributes) ของคลาส เพื่อป้องกันการเข้าถึงหรือลบข้อมูลโดยตรงจากภายนอกคลาส และควบคุมการเข้าถึงข้อมูลเหล่านั้นผ่านเมธอดเฉพาะ

โดยการใช้ **single underscore `(_)`** หรือ **double underscore `(__)`** ในการทำ data hiding เหมือนกับการซ่อน method นั่นเองครับ

1. single underscore `(_)` บ่งบอกว่า method หรือ ตัวแปร เป็น private แต่ยังเข้าถึงได้
2. double underscore `(__)` บ่งบอกว่า method หรือ ตัวแปร ถูกเปลี่ยนชื่ออัตโนมัติ (name mangling) เพื่อป้องกันการเข้าถึงโดยตรงจากภายนอก

```python
class MyClass:
    def __init__(self):
        self.__hidden_data = 42  # ซ่อนข้อมูลนี้ด้วยการใช้ __ (double underscore)
    
    
    def get_hidden_data(self):
        return self.__hidden_data
    
   
    def set_hidden_data(self, value):
        self.__hidden_data = value
        
obj = MyClass()

try:
    print(obj.__hidden_data)  # AttributeError: ไม่มี attribute ชื่อ __hidden_data
except AttributeError as e:
    print(e)  # แสดงข้อผิดพลาด


print("Hidden data:", obj.get_hidden_data())  # แสดงค่า 42
obj.set_hidden_data(99)
print("Updated hidden data:", obj.get_hidden_data())  # แสดงค่า 99

```

จะเห็นว่า double underscore เราไม่สามารถเข้าถึงโดยตรงได้ ต้องใช้ method get กับ set เข้ามาช่วย



## สรุป

method hiding ช่วยในการซ่อน method จากการเข้าถึงผ่าน method ลูก โดย method ของลูก จะทำงานแทน ซ่อน method แม่ คล้ายกับ method overriding และแต่ละภาษาก็จะมีเงื่อนไขหรือวิธีใช้ต่างกัน



## Reference

### การใช้ Method hiding in C# และ อะไรคือ Method hiding

{% embed url="https://www.geeksforgeeks.org/method-hiding-in-c-sharp/" %}

{% embed url="https://www.tutorialsteacher.com/csharp/method-hiding" %}

{% embed url="https://medium.com/@javvadirupasri8/method-hiding-shadowing-and-overriding-in-c-explained-with-examples-643c7dfc8ccc" %}

### static and non-static method มันเกี่ยวกันยังไง ในmethod hiding?

{% embed url="https://www.scaler.com/topics/method-hiding-in-java/" %}

### Method hiding in Java?

{% embed url="https://www.naukri.com/code360/library/method-hiding-in-java" %}

{% embed url="https://www.scaler.com/topics/method-hiding-in-java/" %}

### Method hiding จะมีเมื่อเป็น Static Method

{% embed url="https://dev.to/ravi_sarva/understanding-method-overriding-method-hiding-and-overloading-in-java-53o4" %}

### Data hiding in Python

{% embed url="https://www.javatpoint.com/data-hiding-in-python" %}

{% embed url="https://www.tutorialspoint.com/data-hiding-in-python" %}

### อธิบายการใช้ underscore&#x20;

{% embed url="https://stackoverflow.com/questions/2888035/information-hiding-in-python" %}



