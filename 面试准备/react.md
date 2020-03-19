### React 

1. 虚拟dom ,diff 算法？

2. 合成事件***SyntheticEvent***,实际是事件对象 event；
   1. ***SyntheticEvent***封装了浏览器的原生事件，并对浏览器的做了兼容性处理
   2. ***SyntheticEvent***是池化的，事件对象可以被复用，但是当执行了回调函数之后，事件***event对象***的属性就会被销毁，所以不能在异步中访问合成事件
   
3. React 用单个事件监听器直接监听顶层事件，而不是将单个事件监听器附加到节点上

4. Porps 是外部输入参数，不可变的，State是组件内部参数，控制组件的状态，是可变的

5. super函数的实现？为甚么只有通过父的构造函数才能得到子类的this 对象，同时得到与父类同样的属性和方法。

6. 控制组件：指的是能根据自己输入值进行更新的表单元素

7. 为什么ES6 不提供自动绑定事件监听(addEventListenser)？

8. React Hooks是啥，以及作用？

9. React 中方法绑定this？

10. React.Componnent :作为一个函数接参数 元素h1、元素样式或者类名的组成的对象{className:'class1'}、元素中的文本hello,world

11. 构造函数源码？

12. React Context ?

13. React.createClass ?

14. PureComponent

15. React 虚拟dom 构建已经虚拟dom 转化为真实的dom

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
      4、如果是普通类型的dom 节点，即 html文本，通过type 是小写的字符串判断，遍历props中的普通属性加载到真实的html 标签上，返回真实的dom；
      5、如果type 是一个函数对象，将通过react的createComponent()返回真实的dom；
      6、以对应着 react 中分别处理的四种DOM 对象
      		ReactDOMEmptyComponent:空组件、ReactDOMTextComponent:文本、
    			ReactDOMComponent:原生DOM、ReactCompositeComponent:自定义React组件
    ```

    

16. diff 算法

    >**核心要点**
    >
    >1、采用分层比较，一层一层比较，不能跨级比较；
    >
    >2、采用先序深度优先遍历算法；
    >
    >**步骤**
    >
    >分为三个不同的阶段
    >
    >- tree diff 策略
    >  1. 分层比较，一层一层比较，不同的级别比较久直接删除；
    >- Component diff 策略
    >  1. 组件类型相同，直接采用分层比较的策略；
    >  2. 组件类型不同，直接删除掉旧节点父节点下的全部子节点，用新节点替换；
    >- Element diff 策略
    >  1. 若节点类型不相同，直接用新节点替换旧节点{type:'REPLACE',newNode:newNode}；
    >  2. 若节点类型相同时，直接采用更新的模式，更新相关属性{type:'ATTRS',attrs:{class: 'list-group'};
    >  3. 新节点在旧的节点中不存在，直接在插入新的节点{type:'INSERT',index:XXX};
    >  4. 旧的节点在新的节点中不存在，直接删除掉旧的节点{type:'REMOVE',index:XXX}
