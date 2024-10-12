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



## Reference

### การใช้ Method hiding in C# และ อะไรคือ Method hiding

{% embed url="https://www.geeksforgeeks.org/method-hiding-in-c-sharp/" %}

{% embed url="https://www.tutorialsteacher.com/csharp/method-hiding" %}

{% embed url="https://medium.com/@javvadirupasri8/method-hiding-shadowing-and-overriding-in-c-explained-with-examples-643c7dfc8ccc" %}

### static and non-static method มันเกี่ยวกันยังไง ในmethod hiding?

{% embed url="https://www.scaler.com/topics/method-hiding-in-java/" %}
