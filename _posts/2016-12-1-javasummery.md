---
layout: post
comments: true
title: Java一学期的学习总结
categories: Java
---  

这学期学校开了Java课，但上课的进度太慢，还是习惯了自己看书，书是学校发的《Java语言程序设计（基础篇）》。

### 1.基本数据类型  
Java和C++的基本数据类型都差不多，字节长度也相同，分别为:  

| Byte     | Char  |Int    | Double| Float | Short| Long  |
| :------: |:-----:|:-----:| :----:| :----:|:----:|:-----:|
| 1字节    | 2字节 | 4字节 | 4字节 | 8字节 |2字节 | 8字节 |  

### 2.指针
Java中没有指针，所以要区分在参数传递中是传值还是传引用：对于基本类型，传值；对于对象，传引用(一切继承自Object的类，eg：Collection的子类，String，StringBuffer)，由于String是Char[]的包装类，在参数传递上String表现为传值。  

    public class Main{
     public static void main(String args[]) {
        Integer i = 1;
        String s = "unChanged";
        changeString(s);
        changeInteger(i);
        System.out.println(s+" "+i);
    }
    static void changeString(String s){
        s = "Changed";
    }
    static void changeInteger(Integer i){
        i = 10;
    }
    }   


### 3.泛型
Java中的所有类都继承自超类，所以泛型比起C++满足条件要宽松的多，所以你可以写出很多奇怪的代码：  
比如泛型为Object的List可以储存各种基本数据类型：

    import java.util.ArrayList;
    import java.util.List;
    public class Main{
        public static void main(String args[]) {
            List<Object> list = new ArrayList<>();
            String s = "string";
            int i = 5;
            list.add(s);
            list.add(i);
            for(Object str:list)
                System.out.println(str);
        }
    }  

比如泛型可以有继承关系
//TODO

### 4.Static  
Java中修饰基本数据类型只能用final，即C++中的const，而不能用static来表示一个静态基本数据。static用来在对象中表示静态方法，即不需要实例类就可以使用的方法，所以用static修饰的方法中不能出现super，this，只能使用静态成员变量和静态成员函数。  


### 5.getClass方法
用来返回实例的类，同时给这个实例的类提供了一些方法： 

    public class Main{
        public static void main(String args[]) throws InstantiationException, IllegalAccessException {
            A a = new A();
            System.out.println(a.getClass().getName());
            System.out.println(a.getClass().newInstance().i);
            System.out.println(new A().i);
        }
    }
    class A{
        public int i = 10;
    }
    

### 一些小东西：  
Java的List，Map，Array和C++STL中的差不多，Java多了一些方法（可能是我不知道C++中有），都有 `for(Type s:S)` 和 `Iterator` 。Java不能重载运算符，
  
### 一些小技巧：  
1.Print调试大法好:  

    public class test{
        public static void show(Object o){
                System.out.println(o);
        }
        public static void showArray(String[] s){
            for(Object o:s)
                System.out.print(o+" ");
            System.out.println();
        }
        public static void showCollection(Collection<Object> c){
            for(Object o:c)
                System.out.print(o+" ");
            System.out.println();
        }
    }
    public class Main{
        public static void main(String args[]) {
            int i = 1;
            test.show(1);
            String s = "s";
            test.show(s);
            String[] array = {"1","2"};
            test.showArray(array);
            List list = Arrays.asList(array);
            test.showCollection(list);
        }
    }



