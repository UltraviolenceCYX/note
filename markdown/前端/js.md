## 对于原型继承的理解 

​	所有对象都有一个` __ proto __` 属性，方法对象会多一个prototype属性。一个构造方法的prototype会指向这个构造方法的原型类XXX.prototype,在这个原型类里又会有一个constructor属性指回这个构造方法。构造函数的作用（当new一个xxx（）时）会默认把自己的prototype指向的原型对象赋给新建对象的 ` __ proto __ `属性 。当新建的对象没有一个属性的时候就会顺着`__proto__` 查找有没有这个属性，以完成继承。

​	方法的`__proto__` 是` Function.prototype`

​	后面的Class语法其实就是新建了个constructor，然后把类的属性方法赋值给constructor的`prototype` 对象。

