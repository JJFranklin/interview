基本的用法

参考：https://reacttraining.com/react-router/web/api/Route

中文部分解释：https://juejin.im/post/5be2993df265da611e4d220c 

注意：

> 1、路由将Link 包含在其中；
>
> 2、Switch 将匹配第一个路由组件的内容渲染，其他未匹配的部分不做渲染；
>
> 3、v4 版本之前使用react-router,之后使用 react-routr-dom；
>
> 4、v4 版本之前使用默认路由,防止匹配到主界面，渲染不到子元素,之后使用 react-routr-dom，去掉默认路由，直接用Route 代替；
>
> 5、V4 版本之后，嵌套路由直接写每一层的路由
>
> 嵌套路由：/m/d
>
> V4 版本后 ：
>
> <Route path='/m'>
>
> ​	<Route path='/m/d'></Route>
>
> </Route>
>
> V4 版本前
>
> <Route path='m'>
>
> ​	<Route path='d'></Route>
>
> </Route>
>
> 7、onLeave 和onEnter
>
> 8、浏览器 history 模式
>
> 10、路由的生命周期和组件的生命周期基本一致，渲染组件：componenetDidMount ，离开componentWillUnMount,子路由更新，父组件会重新更新属性。激发 componentWillPropsReceive和componentWillUpdate
>
> 11、最新的react-router-dom 需要将子组件用withRouter(Component)包裹起来，才能拿到history ,location,match 对象,分别对应三种不同的路由获取方式。传参方式：动态路由(/nam/:id)、{pathname:'/name',query:{id:1}}、{pathname:'/name',state:{id:1}}



#### browserHistory、hashHistory、creatememoryhistory 的区别

>1、首先 `browserHistory` 其实使用的是 HTML5 的 `History API`，浏览器提供相应的接口来修改浏览器的历史记录；而 `hashHistory` 是通过改变地址后面的 hash 来改变浏览器的历史记录；
>
>History API 提供了 pushState() 和 replaceState() 方法来增加或替换历史记录。而 hash 没有相应的方法，所以并没有替换历史记录的功能。但 react-router 通过 polyfill 实现了此功能，具体实现没有看，好像是使用 sessionStorage。
>
>2、另一个原因是 hash 部分并不会被浏览器发送到服务端，也就是说不管是请求 `http://domain.com/index.html#foo` 还是 `http://domain.com/index.html#bar` ，服务只知道请求了 index.html 并不知道 hash 部分的细节。而 History API 需要服务端支持，这样服务端能获取请求细节。
>
>3、还有一个原因是因为有些应该会忽略 URL 中的 hash 部分，记得之前将 URL 使用微信分享时会丢失 hash 部分。