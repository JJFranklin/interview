- this 指向

  > this 总是绑定/指向给最后调用它的对象

- 绑定this 的方式

  - 默认绑定

    > 在全局对象中对函数调用，this 默认绑定给window 对象(非严格模式下，严格模式下绑定给undefined)

    ```javascript
    var a = "1";
    function foo(){
      console.log("a:"+this.a);
    }
    foo();// a:1
    // 此时foo()，a 作为window 的对象属性，此时this 默认绑定给window 对象
    ```

  - 隐式绑定

    > 调用函数时，this 绑定给调用函数的对象

    ```javascript
    var a = '2';
    function foo(){
      console.log("a:"+ this.a);
    }
    
    var obj1 = {
      a:3,
      fun1:foo
    }
    
    obj1.fun1();// a:3
    // foo 函数作为obj1属性值，obj1调用函数foo的时候，this 指向obj1 对象
    ```

  - 显示绑定

    > 调用函数时，this 绑定给显示调用的对象

    ```javascript
    var a = '1';
    function foo(){
      console.log("a:"+ this.a);
    }
    
    var obj1 = {
      a:2,
      fun1:foo
    }
    
    var obj2 = {
      a:3,
      fun1:foo
    }
    
    obj1.fun1.call(obj2);// a:3
    obj1.fun1.apply(obj2);// a:3
    // 使用call/apply 显示改变this 绑定指向，this 指向call/apply 的第一个参数
    ```

  - new 方式绑定

    > new 构建新对象的时候，会将this 绑定给新的对象。

    ```javascript
    function foo(){
      this.a = "新的对象";
      console.log(this.a);
    }
    
    var obj1 = new foo();
    obj1.a; // 新的对象
    ```

    构造新对象时，进行如下步骤

    > 1、生成一个新的对象；
    >
    > ２、将this绑定给新的对象；
    >
    > ３、新的对象获得构造函数初始化的属性；
    >
    > 4、返回新的实例化的对象

- apply 和call 

  > 作用:显示改变函数this 的指向

  - apply(obj1,[param1,param2...])

  >apply 接收两个参数，第一个参数是指向的对象，第二个参数是传给调用函数的参数，接收形式的数组 

  - call(obj1,param1,param2...)

  > call 接收多个参数，第一个参数是指向的对象，第二个以及后面的参数是传给调用函数的参数
  >
  > 1、第一个参数传入的是对象，this 指向此对象
  >
  > 2、第一个参数传入的是函数的引用，this 指向此引用
  >
  > 3、第一个参数传入的是null，this 指向window 

- 调用优先级

> new 对象 > 显示调用 > 隐式调用 > 默认调用



PS:箭头函数的绑定无法修改，同时箭头函数this 指向外层的函数或者全局对象，一旦绑定，无法修改