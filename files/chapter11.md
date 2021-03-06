### 模式十一 观察者模式

```go
package main

import (
	"container/list"
)

type Subject interface {
	Attach(Observer) //注册观察者
	Detach(Observer) //释放观察者
	Notify()         //通知所有注册的观察者
}
type Observer interface {
	Update(Subject) //观察者进行更新状态
}

//implements Subject
type ConcreteSubject struct {
	observers *list.List
	value     int
}

func NewConcreteSubject() *ConcreteSubject {
	s := new(ConcreteSubject)
	s.observers = list.New()
	return s
}

func (s *ConcreteSubject) Attach(observe Observer) { //注册观察者
	s.observers.PushBack(observe)
}

func (s *ConcreteSubject) Detach(observer Observer) { //释放观察者
	for ob := s.observers.Front(); ob != nil; ob = ob.Next() {
		if ob.Value.(*Observer) == &observer {
			s.observers.Remove(ob)
			break
		}
	}
}

func (s *ConcreteSubject) Notify() { //通知所有观察者
	for ob := s.observers.Front(); ob != nil; ob = ob.Next() {
		ob.Value.(Observer).Update(s)
	}
}

func (s *ConcreteSubject) setValue(value int) {
	s.value = value
	s.Notify()
}

func (s *ConcreteSubject) getValue() int {
	return s.value
}

/**
 * 具体观察者 implements Observer
 *
 */
type ConcreteObserver1 struct {
}

func (c *ConcreteObserver1) Update(subject Subject) {
	println("ConcreteObserver1  value is ", subject.(*ConcreteSubject).getValue())
}

/**
 * 具体观察者 implements Observer
 *
 */
type ConcreteObserver2 struct {
}

func (c *ConcreteObserver2) Update(subject Subject) {
	println("ConcreteObserver2 value is ", subject.(*ConcreteSubject).getValue())
}

func main() {

	subject := NewConcreteSubject()
	observer1 := new(ConcreteObserver1)
	observer2 := new(ConcreteObserver2)
	subject.Attach(observer1)
	subject.Attach(observer2)
	subject.setValue(5)

}
```

### 总结

>观察者模式一般有三个角色：注册观察者接口、观察者接口、实现注册观察者的类

>注册观察者接口一般有三个方法：1、注册，2、取消注册，3、通知

>