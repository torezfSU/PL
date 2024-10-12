---
description: ประกฤษฎิ์ อรุณกิจเจริญ 650710563
icon: hand-wave
---

# Method Hiding

Method Hiding หมายถึง หมายถึงการซ่อนหรือปกปิด เมธอดของคลาสพ่อแม่ (base class) โดยที่คลาสลูก (derived class) สร้างเมธอดที่มีชื่อเหมือนกันขึ้นมา

แต่ไม่ได้ใช้การสืบทอดผ่านการ **override** แต่เป็นการสร้างเมธอดใหม่ ซึ่งเมธอดในคลาสลูกจะ "ซ่อน" เมธอดของคลาสพ่อแม่เมื่อมีการอ้างอิงหรือสร้างobjectเป็นประเภทของคลาสลูก

ตัวอย่าง C#&#x20;

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
        pRefToC.Print();     
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
