---
layout:     post
title:      "Refactoring"
subtitle:   ""
date:       2017-09-22 12:00:00
author:     "Dingding"
header-img: "img/post/thrift-header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - refactoring 
---

## 为何重构
* 代码结构的流失是累计性的
* 所有事物和行为只表述一次,是优秀设计的根本
* 良好的设计是快速开发的根本
* 如果某一事物或者行为被表述三次及以上,就应该重构
* 不要因为过去的错误设计而懊恼,用重构去弥补
* 最好搭档:一个原作者,一个复审者
* 程序有两面的价值
* 重构和设计彼此互补
* 将数据和对数据的操作行为包装在一起
* 面向对象的一个特征:少用switch
* 难于相与的程序
    * 难以阅读的程序
    * 逻辑重复的程序
    * 添加新行为时需要修改已有代码的程序
    * 带复杂条件逻辑的程序

## 代码的坏味道
* Duplicated Code:重复代码
* Long Method:过长函数
* Large Class:过大类
* Long Parameter List:过长参数列
* Divergent Change:发散式变化--某个类经常因为不同的原因在不同的方向上发生变化
* Shot Surgery:霰弹式变化--某个方向的变化引起在多个不同的类上修改
* Feature Envy : 依恋情节
* Data Clumps:数据泥团--大量数据关联性太强
* Primitive Obsession :基本类型偏执--要有面向对象的思想
* Switch Statements:switch惊悚现身
* Parallel Inheritance Hierachies:平行继承体系
* Lazy Class:冗赘类
* Speculative Generality :夸夸其谈未来性
* Temporary Field :令人迷惑的暂时字段
* Message Chains:过度耦合的消息链
* Middle Man:中间人
* Inappropriate Intimacy:狎昵关系
* Alternative Classes with Different Interfaces:异曲同工的类
* Incomplete Library Class:不完美的类库
* Data Class:纯稚的数据类
* Refused Bequest :被拒绝的遗赠
* Comments:过多注释


## 重新组织函数
* 简短而命名良好的函数
    * 粒度小的函数更有利于复用
    * 粒度小的函数更有利于覆写
    * 命名良好使高层函数易于阅读

* Replace Temp with Query :以查询函数代替临时变量
* Extract Method :提炼函数
以函数名称解释函数用途
* Inline Method :内联函数
* Introduce Explaining Variable :引入解释性变量
将比较复杂的判断条件等表达式简化,并用临时变量指明其意义
* Split Temporary Variable :分解临时变量
临时变量也应该只承担一种职责,不应该赋值为不同的目的赋值
* Remove Assignments to Rarameters:移除对参数的赋值
对形参赋值绝对是一种坏习惯
* Replace Method with Method Object :以函数对象取代函数
函数过于庞杂,局部变量泛滥,将函数转为对象,将局部变量转为对象的字段


## 在对象之间搬移特性
* Move Method :搬移函数
函数不应该与所驻函数之外的函数有更多的交流
使用另一个对象的次数大于使用所在对象的次数
* Move Field:搬移字段
字段被所驻函数之外的另一个类更多的用到
* Extract Class :提炼类
一个类应该只负责一件事,类的功能和大小可能随着时间不断膨胀
* Inline Class:将类内联花
某个类做的事情太少,不能体现其价值
* Hide Delegate :隐藏委托关系
在服务类上建立客户所需的所有函数,用以隐藏委托关系
* Remove Middle Man:移除中间人
某个类做了过多简单的委托动作,应该让客户直接调用受托类
* Introduce Foreign Method :引入外加函数
外加函数用以解决无法修改服务类的情况,属于权宜之计 做法是恢复对象方法的本来面目,将对象作为参数传进函数;除此之外,该函数不应该再有依赖关系
* Introduce Local Extension :引入本地扩展
如果一个类的功能随着时间的积累已经不能完成很多功能,添加了太多的"外加函数",适用于此法建立一个新类,以委托或者继承方式完成原类功能,并加入新功能



## 重新组织数据
* Self Encapsulate Field:自封装字段
不封装的字段可能失去最根本的控制权
* Replace Data Value with Object:以对象取代数据值  
你有一个数据项,需要与其他数据和行为一起使用才有意义
* Change Value to Reference :将值对象改为引用对象
其实就是改为单例模式
* Change Reference To Value:将引用对象改为值对象
值对象应该是不变的.无论何时,只要你调用同一对象的同一查询函数,都应该得到相同的结果;对象不可变含义:可以用另一个同类型的对象取代现有对象,但是本对象依旧不能改变
* Replace Array with Object :以对象取代数组
数组内不应该存放不同类型数据,应该用字段名准确表述字段意义
* Duplicate Observed Data :复制被监视数据
尽管可以轻松的将"行为"划分到不同的部分,"数据"却往往不能如此.同一项数据有可能即需要内嵌于GUI控件,也需要保存于领域模型中.
业务逻辑万万不能被嵌入到用户界面
观察者模式
* Replace Magic Number with Symbolic Constant :以字面常量取代魔法数
魔法数--拥有特殊意义又无法明确表现出这种意义的数字
* Encapsulate Collection
集合的处理方式应该和其他种类的数据略有不同.
取值函数不该返回集合自身,因为这会让用户得以修改集合内容而集合拥有者却一无所悉
* Replace Type Code with Class:以类取代类型码
适用于类型码不会影响宿主的行为
将类型码封装成一个类,用每一个类型码构造出一个对象,供客户端调用
问题根源:任何接受类型码作为参数的函数,所期望的实际上是一个数值,无法强制使用符号名.
任何Switch语句都应该运用Replace Conditional with Polymorphism去掉
* Replace Type Code with Subclasses:以子类取代类型码
Replace Conditional with Polymorphism 的基础
使用该方法可以将"只有特定类型码相关的行为"转移到更具体的类中,以彰显这一事实
如果新增了类型码,只需新增一个类,极为方便
swith语句应只用于决定创建何种类
  
## 简化条件表达式
* Decompose  Conditional:分解条件表达式
大型函数自身就会使代码的可读性下降,条件逻辑会使代码更难阅读
复杂的条件逻辑是最常导致复杂度上升的地点之一

* Consolidate Conditional Expression:合并条件表达式
检查条件各不相同,最终行为却一致
* Consolidate Duplicate Conditional Fragments:合并重复的条件分支片段
可以清楚地表明哪些随条件变动,哪些不随条件变动
* Remove Control Flag:移除控制标记
以break语句或者return语句取代控制标记
* Replace Nested Conditional with Guard Clauses
使用卫语句展现出所有特殊的情况; 条件表达式通常有两类表现方式:所有分支都属于正常行为;只有一个分支是正常行为
* Replace Conditional with Polymorphism:以多态取代条件表达式
主要指switch语句
* Introduce Null Object:引入Null对象
* Introduce Assertion:引入断言

## 杂项
* 整洁的代码只做好一件事
* 表述应该只出现一遍
* 类和函数应该足够小,以消除标识符前缀.
* if else while 语句等,其中的代码块应该只有一行.该行大抵是个函数调用语句
* 每结构化编程规则：每个函数，函数中的每个代码块都应该只有一个入口、一个出口;但是实践表明,更重要的在于保持函数体短小精悍，这时使用多个return ，break等可以更有表达力
