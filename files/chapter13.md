### 模式十三 适配器
```go
package main

import "fmt"

//鸭接口
type Duck interface {
	fly()
	quack()
}

//火鸡接口
type Turkey interface {
	gobble()
	fly()
}

//具体的鸭
type MallardDuck struct {
}

func (this *MallardDuck) fly() {
	fmt.Println("mallardDuck can fly")
}

func (this *MallardDuck) quack() {
	fmt.Println("mallardDuck can quack")
}

//具体火鸡
type WildTurkey struct {
}

func (this *WildTurkey) gobble() {
	fmt.Println("WildTurkey can gobble")
}

func (this *WildTurkey) fly() {
	fmt.Println("WildTurkey can fly")
}

//因为包装了鸭子适配器的火鸡，其外表函数是鸭子，但是实际调用了火鸡方法
type TurkeyAdapter struct {
	turkey Turkey
}

func NewTurkeyAdapter(turkey Turkey) *TurkeyAdapter {
	return &TurkeyAdapter{turkey: turkey}
}

func (this *TurkeyAdapter) fly() {
	this.turkey.fly()
}

func (this *TurkeyAdapter) quack() {
	this.turkey.gobble()
}

func main()  {
  t := new(WildTurkey)
  d := NewTurkeyAdapter(t)
  
  d.fly()
  d.quack()
}

```

#### 定义
>适配器模式(Adapter Pattern) ：将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器(Wrapper)。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。

#### 结构
* Target：目标抽象类
* Adapter：适配器类
* Adaptee：适配者类
* Client：客户类

>对象适配器

![](https://github.com/developersPHP/design-patterns-go/blob/master/images/Adapter.jpg)

>类适配器

![](https://github.com/developersPHP/design-patterns-go/blob/master/images/Adapter_classModel.jpg)

>时序图

![](https://github.com/developersPHP/design-patterns-go/blob/master/images/seq_Adapter.jpg)

#### 优点
* 将目标类和适配者类解耦，通过引入一个适配器类来重用现有的适配者类，而无须修改原有代码
* 增加了类的透明性和复用性，将具体的实现封装在适配者类中，对于客户端类来说是透明的，而且提高了适配者的复用性
* 灵活性和扩展性都非常好，通过使用配置文件，可以很方便地更换适配器，也可以在不修改原有代码的基础上增加新的适配器类，完全符合“开闭原则”

>类适配器模式还具有如下优点：
 由于适配器类是适配者类的子类，因此可以在适配器类中置换一些适配者的方法，使得适配器的灵活性更强

>对象适配器模式还具有如下优点：
 一个对象适配器可以把多个不同的适配者适配到同一个目标，也就是说，同一个适配器可以把适配者类和它的子类都适配到目标接口。
 
#### 缺点
>类适配器模式的缺点如下：
 对于Java、C#等不支持多重继承的语言，一次最多只能适配一个适配者类，而且目标抽象类只能为抽象类，不能为具体类，其使用有一定的局限性，不能将一个适配者类和它的子类都适配到目标接口。
 
>对象适配器模式的缺点如下：
 与类适配器模式相比，要想置换适配者类的方法就不容易。如果一定要置换掉适配者类的一个或多个方法，就只好先做一个适配者类的子类，将适配者类的方法置换掉，然后再把适配者类的子类当做真正的适配者进行适配，实现过程较为复杂
 

