---
layout: post
title: 设计模式之 迭代器模式（Iterator Pattern） Java实现
date: 2019-02-07 20:43:23.000000000 +09:00
category: Design Pattern
---

所谓迭代器模式，就是把集合的数据和遍历分开

让遍历集合依赖于迭代器，而不依赖于具体的集合

从而可以达到代码复用

java中的增强for循环就是迭代器模式的应用，要使用增强for循环语法的集合，就要实现java.util.Iterator接口。

在迭代器模式中，需要几个角色

* Iterator(迭代器)
* ConcreteIterator(具体的迭代器)
* Aggregate(集合)
* ConcreteAggregate(具体的集合)
它们的类图是这样的：
![类图](/assets/images/IteratorPatternImg1.png)



下面用一个具体的例子来理解迭代器模式



用迭代器模式实现对书架的遍历

根据上面迭代器模式的类图，书架和迭代器需要以下：


|Book	| 书的类 |
| ------ | ------ |
|BookShelf	|书架的类|
|BookShelfIterator|	书架的迭代器|
|Iterator	|迭代器的接口|
|Aggregate	|集合的接口|


类图如下：
![类图2](/assets/images/IteratorPatternImg2.png)



代码实现如下

书的类：

```java
package cn.wsichao.iteratorPattern;

public class Book {
    private String name;

    public Book(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
集合接口：

```java
package cn.wsichao.iteratorPattern;

public interface Aggregate {
    public abstract Iterator iterator();
}
迭代器接口：

package cn.wsichao.iteratorPattern;

public interface Iterator {
    boolean hasNext();
    Object next();
}
```
书架类：

```java
package cn.wsichao.iteratorPattern;

public class BookShelf implements Aggregate {
    private Book[] books;
    private int last = 0;

    public BookShelf(int maxsize) {
        this.books = new Book[maxsize];
    }

    public Book getBookAt(int index){
        return books[index];
    }

    public void appendBook(Book book){
        this.books[last] = book;
        last++;
    }

    public int getLength(){
        return last;
    }

    @Override
    public Iterator iterator() {
        return new BookShelfIterator(this);
    }
}
```
书架的迭代器类：

```java
package cn.wsichao.iteratorPattern;

public class BookShelfIterator implements Iterator {
    private BookShelf bookShelf;
    private int index;

    public BookShelfIterator(BookShelf bookShelf) {
        this.bookShelf = bookShelf;
        this.index = 0;
    }

    @Override
    public boolean hasNext() {
        if(index < bookShelf.getLength()){
            return true;
        } else{
            return false;
        }
    }

    @Override
    public Object next() {
        Book book = bookShelf.getBookAt(index);
        index++;
        return book;
    }
}
```

最后测试一下：

```java
package cn.wsichao.iteratorPattern;

public class main {
    public static void main(String[] args) {
        BookShelf bookShelf = new BookShelf(5);

        bookShelf.appendBook(new Book("java"));
        bookShelf.appendBook(new Book("c"));
        bookShelf.appendBook(new Book("C++"));
        bookShelf.appendBook(new Book("python"));

        Iterator iterator = bookShelf.iterator();
        while(iterator.hasNext()){
            Book book = (Book) iterator.next();
            System.out.println(book.getName());
        }
    }
}
```

结果：


>java  
>c  
>C++  
>python  
>
>Process finished with exit code 0


