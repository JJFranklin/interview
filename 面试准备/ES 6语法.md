### **解构赋值**

```javascript
// 数组
let s = [1,2,3,...[4,5,6]];// [1,2,3,45,6]
// 对象
let {x:a,y:b,z:c} = {a:1,b:3,c:5};// x:1,y:3,z:4
// 如果参数和属性名一致，可以简写为
let {a,b,c} = {a:1,b:2,c:3}

// 用于交换数据
let [x,y] = [1,2];
[x,y] = [y,x]
[x,y] // 2,1
```

##### 注意点

> 没有var、const、let 等标识符是，对象解构必须用（）包裹起来;
>
> ([x,y] = [1,2])



### Iterator 遍历器

提供不同数据解构同一访问的接口，能够遍历数据解构中的成员。

```latex
原生具备 Iterator 接口的数据结构如下
- Array
- Map
- Set
- String
- TypedArray
函数的 arguments 对象
可以使用for...of 遍历他的成员，即遍历值。
【for...in 是遍历键】
```



### Set 和Map

##### Set：成员唯一的数组。

Set 方法

```javascript
1、set.add(val)//添加数据
2、set.delete(val) //删除某个值
3、set.has(val) // 判断是否有这个值
4、set.size // 获取长度
5、set.clear() // 清除所有数据
```

#####Map：提供多种键的javaScript 对象。

> 原来的javaScript 对象，键-值对形式，键只能是字符串，Map扩充了，任何值都可以。

Map 方法

```javascript
1、map.set(key,val) // 添加数据
2、map.get(key);// 获得数据
3、map.delete(key); // 删除数据
4、map.claer() // 清除所有数据
```

##### Map 和 Set 共有的方法

````javascript
— keys()：返回键名的遍历器。
— values()：返回键值的遍历器。
— entries()：返回所有成员的遍历器。
— forEach()：遍历成员。
````



### 数组

#### 继承

ES5 的继承

1、父类new一个实例对象；

2、this指向新的实例对象；

3、将父类属性添加到子类实例对象上；

ES6 的继承

1、先用super 实现一个父类对象的this；

2、再通过子类的构造函数修饰this，让this指向子类实例；

3、子类就可以继承父类的全部属性

![image-20200307172735213](/Users/franklin/Library/Application Support/typora-user-images/image-20200307172735213.png)



