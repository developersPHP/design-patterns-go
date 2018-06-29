### 模式十四 桥接模式
```go
package main

import "fmt"

//不同的图案有不同颜色描述，其实这个设计就可以抽象成桥接模式
//对图案抽象，对颜色是实现，不同图案组合不同颜色

type Pattern interface {
	showColor()
}

type Color interface {
	setColor()
	getColor() string
}

//圆形
type Circular struct {
	col Color
}

func (this *Circular) showColor() {
	this.col.setColor()
	fmt.Printf("圆形的颜色是%s\n", this.col.getColor())
}

//方形
type Square struct {
	col Color
}

func (this *Square) showColor() {
	this.col.setColor()
	fmt.Printf("方形的颜色是%s\n", this.col.getColor())
}

type Yellow struct {
	color string
}

func (this *Yellow) setColor() {
	this.color = "黄色"
}

func (this *Yellow) getColor() string {
	c := this.color
	return c
}

type White struct {
	color string
}

func (this *White) setColor() {
	this.color = "白色"
}

func (this *White) getColor() string {
	c := this.color
	return c
}

func main() {
	y := new(Yellow)
	c := &Circular{col: y}
	c.showColor()
	w := new(White)
	q := &Square{col: w}
	q.showColor()
}

```

#### 适用性
* 如果一个系统需要在构件的抽象化角色和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的继承联系，通过桥接模式可以使它们在抽象层建立一个关联关系。
* 抽象化角色和实现化角色可以以继承的方式独立扩展而互不影响，在程序运行时可以动态将一个抽象化子类的对象和一个实现化子类的对象进行组合，即系统需要对抽象化角色和实现化角色进行动态耦合
* 一个类存在两个独立变化的维度，且这两个维度都需要进行扩展。
* 虽然在系统中使用继承是没有问题的，但是由于抽象化角色和具体化角色需要独立变化，设计要求需要独立管理这两者。
* 对于那些不希望使用继承或因为多层次继承导致系统类的个数急剧增加的系统，桥接模式尤为适用

#### 定义
>桥接模式(Bridge Pattern)：将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式，又称为柄体(Handle and Body)模式或接口(Interface)模式。

#### 结构
* Abstraction：抽象类
* RefinedAbstraction：扩充抽象类
* Implementor：实现类接口
* ConcreteImplementor：具体实现类

![](https://github.com/developersPHP/design-patterns-go/blob/master/images/Bridge.jpg)

>时序图

![](https://github.com/developersPHP/design-patterns-go/blob/master/images/seq_Bridge.jpg)

#### 优点

* 分离抽象接口及其实现部分。
* 桥接模式有时类似于多继承方案，但是多继承方案违背了类的单一职责原则（即一个类只有一个变化的原因），复用性比较差，而且多继承结构中类的个数非常庞大，桥接模式是比多继承方案更好的解决方法。
* 桥接模式提高了系统的可扩充性，在两个变化维度中任意扩展一个维度，都不需要修改原有系统。
* 实现细节对客户透明，可以对用户隐藏实现细节。

#### 缺点
* 桥接模式的引入会增加系统的理解与设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计与编程。桥接模式要求正确识别出系统中两个独立变化的维度，因此其使用范围具有一定的局限性
