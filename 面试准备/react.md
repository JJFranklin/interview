### React 

1. 虚拟dom ,diff 算法？

2. 合成事件***SyntheticEvent***,实际是事件对象 event；
   1. ***SyntheticEvent***封装了浏览器的原生事件，并对浏览器的做了兼容性处理
   2. ***SyntheticEvent***是池化的，事件对象可以被复用，但是当执行了回调函数之后，事件***event对象***的属性就会被销毁，所以不能在异步中访问合成事件
   
3. React 用单个事件监听器直接监听顶层事件，而不是将单个事件监听器附加到节点上

4. Porps 是外部输入参数，不可变的，State是组件内部参数，控制组件的状态，是可变的

5. super函数的实现？为甚么只有通过父的构造函数才能得到子类的this 对象，同时得到与父类同样的属性和方法。

   > super 代表父类的构造函数
   >
   > **子类必须在constructor方法中调用super方法，否则新建实例时会报错。**
   >
   > **这是因为子类没有自己的this对象，而是继承父类的this对象**
   >
   > **，然后对其进行加工。如果不调用super方法，子类就得不到this对象。
   > **
   >
   > **ES5的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)）。ES6的继承机制完全不同，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。
   > **
   >
   > **在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错。这是因为子类实例的构建，是基于对父类实例加工，只有super方法才能返回父类实例。**

6. 控制组件：数据源来自于state;自己维护状态； 并根据自己输入值进行更新的表单元素；

7. 非受控组件：可以将值绑定到dom 节点上

   ```react
   constructor(props){
     super(props);
     this.input = React.createRef();
   }
   handleChange(){
     let inputValue = this.input.current.value;// 不依靠状态获取表单的值
   }
   // 组件部分
   <input ref={this.input} onChange={this.handleChange.bind(this)}></input>
   ```

   

8. 为什么ES6 不提供自动绑定事件监听(addEventListenser)？

9. React Hooks是啥，以及作用？

   > useState:在函数组件中，成对初始化状态和设置更新状态函数的hook 
   >
   > [number,setNumber] = useState(0)
   >
   > 类似于生命了 state和setState;
   >
   > useEffect:相当于是结合了React 的加载，更新，卸载三个生命周期，在每一次状态更新之后，调用此函数。当然也可以控制何时更新。

10. React 中方法绑定this？

11. React.Component :作为一个函数接参数 元素h1、元素样式或者类名的组成的对象{className:'class1'}、元素中的文本hello,world

12. 构造函数源码？

13. React Context :提供了一个跨组件传递数据的方式，并通过传递函数，更新state,进而更新context的值。

14. React.createClass :React 声明组件的另一种方式，和class extends 方式主要区别在属性和状态初始化上，同时他能很好的绑定this。

15. PureComponent：PureComponen和Component 相比，shouldComponentUpdate 函数只是浅层次的实现了此函数，因此PureComponent并不需要显式写出此函数，只有有更新就会更新组件

16. React 虚拟dom 构建已经虚拟dom 转化为真实的dom

    ```javascript
    1、所有的jsx 编写的react 组件，都会经过babel转化为javaScript对象，如下所示
    /*
    'div',
     { className: 'cn' },
     'Content!'
    */
    2、React.CreatElement(type, props, children)接收上述对象，通过下列步骤转化为虚拟dom
    	1、收集属性对象props,将特殊的属性key、children、ref 等进行特殊处理，合并为一个新的属性；
      2、将生成的最新的属性和tye 传入到reactElement(type,newProps)，返回一个全新的newElement;
    	3、虚拟dom 创建成功.
    3、通过React 的render()将虚拟Dom 转化为真实的dom
    	1、接收虚拟dom,判断虚拟dom 的类型；
      2、null或者false，直接返回为空，不渲染；
      3、如果是基本文本类型的，通过creatTextNode()返回文本类型的节点；
      4、如果是普通类型的dom节点，即原始dom节点，通过type为小写的是原始dom节点，遍历props中的普通属性加载到真实的dom标签上，返回真实的dom；
      5、如果type是一个函数对象或者是大写，即为自定义组件，将通过react的createComponent()返回真实的dom；
      6、以对应着 react 中分别处理的四种DOM 对象
      		ReactDOMEmptyComponent:空组件、ReactDOMTextComponent:文本、
    			ReactDOMComponent:原生DOM、ReactCompositeComponent:自定义React组件
    ```

    

17. diff 算法

    >**核心要点**
    >
    >1、采用分层比较，一层一层比较，极少跨级比较；
    >
    >2、约定不同类型的组件，结构极少相似；
    >
    >3、同一层级的子节点，可以使用唯一的id 进行区分；
    >
    >4、采用先序深度优先遍历算法；
    >
    >**步骤**
    >
    >分为三个不同的阶段
    >
    >- tree diff 策略
    > 1. 分层比较，一层一层比较，不同的级别比较，就直接删除；
    >- Component diff 策略
    > 1. 组 件类型相同，直接采用分层比较的策略；
    > 2. 组件类型不同，直接删除掉旧节点父节点下的全部子节点(即当前的旧节点)，用新节点替换；
    >- Element diff 策略
    > 1. 若节点类型不相同，直接用新节点替换旧节点{type:'REPLACE',newNode:newNode}；
    > 2. 若节点类型相同时，直接采用更新的模式，更新相关属性{type:'ATTRS',attrs:{class: 'list-group'};
    > 3. 新节点在旧的节点中不存在，直接在插入新的节点{type:'INSERT',index:XXX};
    > 4. 旧的节点在新的节点中不存在，直接删除掉旧的节点{type:'REMOVE',index:XXX}
    >
    >__Key__ 的作用
    >
    >1、迅速判断节点是否存在；
    >
    >2、减少遍历次数，提高diff 的效率
    >
    >3、但是key ，不一定会提高效率，简单的情况下可以不使用key 
    >
    >灵活的销毁重建组件：一般很少使用
    >
    >1、添加唯一的key;
    >
    >2、渲染不同类型的组件

18、setState 源码=> 浅复制和深复制

https://zhuanlan.zhihu.com/p/20328570

> 深复制：完全copy 一份副本，修改副本的值对原来的值 没有影响
>
> 浅复制：copy 一份副本，修改副本中值如果是基本的数据类型，不会影响原始值，如果修改对象类型，会影响到原始的数据类型
>
> __深拷贝方式__
>
> 1、JSON序列化和反序列化：function 等一些会丢失，无法解决相互引用；
>
> 2、循环迭代遍历对象，对于对象和数组来说，要注意区分二者，然后需要遍历自身的属性，不需要原型链上的；
>
> __浅拷贝方式__
>
> 1、object.assign
>
> 2、slice
>
> 3、concat

19、React 基于路由进行代码分割:使用 Lazy 和react-router 的模块,懒加载路由模块；

20、export default 和 modules export的区别

21、context :无需为每个组件手动添加props,使得组件树之间能够共享数据的方式。

​		注意是共同使用同一些数据。同时也避免了，props drilling（属性穿透）

22、虚拟化长列表：[react-window](https://react-window.now.sh/) 和 [react-virtualized](https://bvaughn.github.io/react-virtualized/) 是热门的虚拟滚动库

23、为什么 在调用 `super()` 方法之前，子类构造函数无法使用`this`引用？

25、为什么类方法需要绑定到类实例？

> 1、*类声明*和*类表达式*的主体以 [严格模式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) 执行，主要包括构造函数、静态方法和原型方法。Getter 和 setter 函数也在严格模式下执行。
>
> 2、在类中声明的函数如果没有显示绑定的this对象，在函数调用的时候就会报错，显示为 undefined，按照js 的指向原则，应该是指向调用的对象或者上下文对象，此时的上下文对象是react 的实例对象，默认绑定到react的实例对象，this应该不会是undefined。为什么？因为1中的原因，严格模式下，不会进行默认绑定，此时需要显示绑定this。
>
> 3、深层次的原因是js参数赋值转换的时候，this 会丢失
>
> ​	var obj = {
>
> ​			name:'jack',
>
> ​			getName:function(){
>
> ​					console.log(this.name);
>
> ​			}
>
> ​	}
>
> var func1 = obj.getName;
>
> func1(); // undefined 此时this 在赋值过程中，就丢失了
>
> 在React 对事件的处理中，也有一个赋值转换的过程，导致this丢失了，所以需要手动绑定this



26、浅比较

> 1、比较的方式，主要采用了es6 中的objec.is() 的方式
>
> 2、主要比较基本数据类型，如果对象有多层嵌套，只会比较嵌套对象的第一层，不会循环递归比价。就造成了有时候使用PureComponent 也不会减少性能，属性更新发现组件没有改变的情况；
>
> 3、object.is() 只能对基本数据类型进行精确比较，对象比较需要采用步骤2中的方式；







