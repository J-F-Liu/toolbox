# React

## 官方文档
- https://facebook.github.io/react/docs/getting-started.html
- https://facebook.github.io/react/blog/
- [React Components, Elements, and Instances](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)
- [React.cloneElement](https://facebook.github.io/react/blog/2015/03/03/react-v0.13-rc2.html#react.cloneelement)
- [Mixins Considered Harmful](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html)

## 学习资料

- [走进前端开发之：框架的演变](http://mp.weixin.qq.com/s?__biz=MzAwMjMxNzQ0MQ==&mid=400374759&idx=1&sn=78830f8b92d7ae4f0f6d40aeee1ecd16#rd)
- [React’s JSX: The Other Side of the Coin](https://medium.com/@housecor/react-s-jsx-the-other-side-of-the-coin-2ace7ab62b98)
- [You’re Missing the Point of React](https://medium.com/@dan_abramov/youre-missing-the-point-of-react-a20e34a51e1a)
- [Navigating the React.JS Ecosystem](https://www.toptal.com/react/navigating-the-react-ecosystem)
- [React组件的生命周期](https://segmentfault.com/a/1190000006807631)
- [ReactJS组件间沟通的一些方法](http://www.alloyteam.com/2016/01/some-methods-of-reactjs-communication-between-components/)
- [React Router 使用教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html)
- [前后端分离系列文章](http://bbear.me/tag/qian-hou-duan-fen-chi/)
- [ES2015实战——面向未来编程](http://yanhaijing.com/javascript/2016/04/27/es2015-practice/)
- [React 相关经验及发展动态](https://zhuanlan.zhihu.com/purerender)
- [高性能 React 组件](http://taobaofed.org/blog/2016/08/12/optimized-react-components/)
- [React入门与进阶之路由](http://blog.codingplayboy.com/2016/10/24/react_router/)

## 范例代码

- [webpack-react-boilerplate](https://github.com/J-F-Liu/webpack-react-boilerplate)

## JSX的优点
React’s JSX is fundamentally superior for a few simple reasons:
- Leverage the Full Power of JavaScript
- Compile-time Errors
- Intellisense Support

## 基本概念
An element is a plain object describing what you want to appear on the screen in terms of the DOM nodes or other components.
Elements can contain other elements in their props. Creating a React element is cheap. Once an element is created, it is never mutated.

A component can be declared in several different ways. It can be a class with a render() method.
Alternatively, in simple cases, it can be defined as a function. In either case, it takes props as an input, and returns an element tree as the output.

When a component receives some props as an input, it is because a particular parent component returned an element with its type and these props.
This is why people say that the props flows one way in React: from parents to children.

An instance is what you refer to as this in the component class you write. It is useful for storing local state and reacting to the lifecycle events.
Functional components don’t have instances at all. Class components have instances, but you never need to create a component instance directly—React takes care of this.
