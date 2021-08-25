---
layout: post
title: Java设计模式学习——建造者模式
toc: true
reward: false
date: 2021-08-09 14:25:17
tags: [设计模式]
categories: [Java]
---
## 建造者模式
>建造者模式（Builder Pattern）使用多个简单的对象构建一个复杂的对象，这种类型的设计模式就属于创建型模式，它提供了一种创建对象的最佳方式。
一个建造者（Builder）类会一步一步构造最终的对象，该 Builder 类是独立于其他的对象的
<!-- more -->
****
### 介绍
* **优点**
  * 建造者独立，易扩展
  * 便于控制细节风险
* **缺点**
  * 产品有其共同点，范围有限制
  * 如内部变化复杂，会有很多的建造者
* **使用场景**
  * 需要生产的对象具有复杂的内部结构，
  * 需要生成的对象内部属性本身相互依赖
* ### 注意事项
  * 与工厂模式的区别L建造者模式更加关注与零件装配的顺序
  ***
### 实现
我们以肯德基套餐为例
我们假设一个快餐店的商业案例，其中，一个典型的套餐可以是一个汉堡（Burger）和一杯冷饮（Cold drink）。汉堡（Burger）可以是素食汉堡（Veg Burger）或鸡肉汉堡（Chicken Burger），它们是包在纸盒中。冷饮（Cold drink）可以是可口可乐（coke）或百事可乐（pepsi），它们是装在瓶子中。

#### **步骤一**
  创建一个表示食物条目和食物包装的接口
  * 食物条目接口
  ```
  /**
   * 一个表示食物条目
   */
  public interface Item {
    public String name();
    public Packing packing();
    public float price();
  }
  ```
  * 食物包装接口
  ```
  /**
   * 食物包装的接口
   */
  public interface Packing {
    public String pack();
  }
  ```
#### **步骤二**
  创建实现 **Packing** 接口实体类
  * Wrapper（包装纸） 
  ```
  public class Wrapper implements Packing {

    @Override
    public String pack() {
        return "Wrapper";
    }
  }
  ```
  * Bottle（瓶子）
  ```
  public class Bottle implements Packing {
    @Override
    public String pack() {
        return "Bottle";
    }
  }
  ```
#### **步骤三**
  创建实现**Item**接口的抽象类，该类提供默认的功能
  * Burger (汉堡）
   ```
   public abstract class Burger implements Item {

      @Override
       public Packing packing() {
        return new Wrapper();
      }

      public abstract float price() ;
   }
   ```
  * ColdDrink （冷饮）
  ```
  public abstract class ColdDrink implements Item {

    @Override
    public Packing packing() {
        return new Bottle();
    }

    public abstract float price();
  }
  ```
***
#### 步骤四
创建扩展了 Burger 和 ColdDrink 的实体类
  * VegBurger （蔬菜汉堡）
  ```
  public class VegBurger extends Burger{
    @Override
    public String name() {
        return "Veg Burger";
    }

    @Override
    public float price() {
        return 25.0f;
    }
  }
  ```
***
  *  ChickenBurger (鸡肉汉堡)
  ```
  public class ChickenBurger extends Burger{
    @Override
    public String name() {
        return "Chicken Burger";
    }

    @Override
    public float price() {
        return 50.5f;
    }
  }
  ```

  * Coke ( 可口可乐)
  ```
  public class Coke extends ColdDrink{
    @Override
    public String name() {
        return "Coke";
    }

    @Override
    public float price() {
        return 30.0f;
    }
  }
  ```

  * Pepsi (百事可乐)
  ```
  public class Pepsi extends ColdDrink{
    @Override
    public String name() {
        return "Pepsi";
    }

    @Override
    public float price() {
        return 35.0f;
    }
  }
  ```
#### 步骤五
创建一个套餐**meal**类，带有上面定义的**Item**对象
```
public class Meal {
    private List<Item> items = new ArrayList<>();

    public void addItem(Item item) {
        items.add(item);
    }

    public float getCost() {
        float cost = 0.0f;
        for (Item item : items) {
            cost += item.price();
        }
        return cost;
    }

    public void showItems() {
        for (Item item : items) {
            System.out.println("Item : " + item.name() + ", Packing : " + item.packing().pack() + ", Price : " + item.price());
        }
    }
}
```
#### 步骤六
创建一个构造者**MealBuilder**类，实例化`builde`类负责创建的**Meal**套餐对象
```
public class MealBuilder {
    public Meal prepareVegMeal(){
        Meal meal = new Meal();
        meal.addItem(new VegBurger());
        meal.addItem(new Coke());
        return meal;
    }

    public Meal prepareNonVegMeal(){
        Meal meal = new Meal();
        meal.addItem(new ChickenBurger());
        meal.addItem(new Pepsi());
        return meal;
    }
}
```
#### 步骤七
使用**MealBuilder**演示构建者模式（Builder Pattern）
```
public class Test {
    public static void main(String[] args) {
        MealBuilder mealBuilder = new MealBuilder();

        Meal vegMeal = mealBuilder.prepareVegMeal();
        System.out.println("Veg Meal");
        vegMeal.showItems();
        System.out.println("Toast Cost:"+vegMeal.getCost());

        Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
        System.out.println("Non-veg Meal");
        nonVegMeal.showItems();
        System.out.println("Toast Cost: "+nonVegMeal.getCost());
    }
}
```
#### 步骤八
执行程序输出结果
>Veg Meal
Item : Veg Burger, Packing : Wrapper, Price : 25.0
Item : Coke, Packing : Bottle, Price : 30.0
Toast Cost:55.0
Non-veg Meal
Item : Chicken Burger, Packing : Wrapper, Price : 50.5
Item : Pepsi, Packing : Bottle, Price : 35.0
Toast Cost: 85.5

