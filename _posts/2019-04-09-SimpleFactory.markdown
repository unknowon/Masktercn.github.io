---
layout: post
title: 设计模式之 简单工厂模式（Simple Factory） Java实现
date: 2019-04-09 20:58:23.000000000 +08:00
tags: Design Pattern
---

#设计模式之 简单工厂模式（Simple Factory）

传入一个值，得到一个产品

这个就是简单工厂模式，省略了中间这个产品生成的过程

举个栗子

我说要有水，于是就有了水

至于水是怎么来的，是大自然的搬运工搬来的还是天上下的都不重要，反正就是得到了水

然后我再说要有面包，于是就有了面包，至于面包是怎么做出来的我并不知道。

这个就很像是在餐馆里点菜，你告诉服务员，你需要  蒸羊羔,蒸熊掌,蒸鹿尾儿,烧花鸭,烧雏鸡儿,烧子鹅,卤煮咸鸭

服务员就给你上了这些菜，但是后厨的加工过程你并不清楚。

---

在简单工厂模式里有几个角色

工厂（Creator）角色

抽象产品（Product）角色

具体产品（Concrete Product）角色&nbsp;&nbsp;

&nbsp;&nbsp;

而用上面点菜的栗子来说就相当于 

餐馆  ==  工厂

菜名  ==  抽象产品

吃到嘴里的菜  ==  具体产品



下面用这个栗子来写个小程序

首先我们需要一个餐馆

```java
public class RestaurantFacroty {
	private Dish dish;
	public void order(String dishName){
		if(dishName.equals("SteamedBearsPaw")){
			dish = new SteamedBearsPaw();
			dish.cook();
		} else if(dishName.equals("SteamedLamb")){
			dish = new SteamedLamb();
			dish.cook();
		} else{
			System.out.println("客官，没有这个菜呀");
		}
	}
}
```


接下来，我们来两个菜

他们需要实现同一个接口

```java
public interface Dish {
	public void cook();
}
```
  蒸熊掌

```java
public class SteamedBearsPaw implements Dish {
	@Override
	public void cook() {
		System.out.println("蒸熊掌上菜啦");
	}
}
```


  蒸羊羔

```java
public class SteamedLamb implements Dish {
	@Override
	public void cook() {
		System.out.println("蒸羊羔上菜啦");
	}
}
```



好了，接下来王某人来点餐了

```java
	public class WangMouRen {
		public static void main(String[] args) {
			RestaurantFacroty restaurant = new RestaurantFacroty();
			restaurant.order("SteamedBearsPaw");
			restaurant.order("SteamedLamb");
			restaurant.order("SteamedDeerTail");
		}
	}
```
我告诉餐馆，我要一个蒸熊掌、蒸羊羔、蒸鹿尾儿，餐馆就给我上了菜，没有蒸鹿尾儿所以说了没有这个菜

好了。这个就是简单工厂模式（Simple Factory）