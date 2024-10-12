---
description: ประกฤษฎิ์ อรุณกิจเจริญ 650710563
---

# Method Hiding

&#x20;    Method Hiding หมายถึง หมายถึงการซ่อนหรือปกปิด เมธอดของคลาสพ่อแม่ (base class) โดยที่คลาสลูก (derived class) สร้างเมธอดที่มีชื่อเหมือนกันขึ้นมา&#x20;

&#x20;   แต่ไม่ได้ใช้การสืบทอดผ่านการ **override** แต่เป็นการสร้างเมธอดใหม่ ซึ่งเมธอดในคลาสลูกจะ "ซ่อน" เมธอดของคลาสพ่อแม่เมื่อมีการอ้างอิงหรือสร้างobjectเป็นประเภทของคลาสลูก



<pre class="language-csharp" data-full-width="false"><code class="lang-csharp"><strong>class Parent
</strong><strong>{
</strong><strong>    public void Print()
</strong><strong>    {
</strong>        Console.WriteLine("Parent class Print method");
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

        p.Print();           // ผลลัพธ์: Parent class Print method
        p.Display();         // ผลลัพธ์: Parent class Display method
        c.Print();           // ผลลัพธ์: Child class Print method
        c.Display();         // ผลลัพธ์: Parent class Display method (เพราะไม่ได้ซ่อน)
        pRefToC.Print();     // ผลลัพธ์: Parent class Print method
    }
}
</code></pre>



