# Reactivity

[A Hands-on Introduction to Fine-Grained Reactivity](https://dev.to/ryansolid/a-hands-on-introduction-to-fine-grained-reactivity-3ndf)
[Building a Reactive Library from Scratch](https://dev.to/ryansolid/building-a-reactive-library-from-scratch-1i0p)

## 安装
```
yarn add react react-dom prop-types
yarn global add react-devtools
react-devtools
```

## 官方文档
- https://facebook.github.io/react/docs/getting-started.html
- https://facebook.github.io/react/blog/
- [React Components, Elements, and Instances](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)
- [React.cloneElement](https://facebook.github.io/react/blog/2015/03/03/react-v0.13-rc2.html#react.cloneelement)
- [Mixins Considered Harmful](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html)

## 学习资料

- [走进前端开发之：框架的演变](http://mp.weixin.qq.com/s?__biz=MzAwMjMxNzQ0MQ==&mid=400374759&idx=1&sn=78830f8b92d7ae4f0f6d40aeee1ecd16#rd)
- [React’s JSX: The Other Side of the Coin](https://medium.com/@housecor/react-s-jsx-the-other-side-of-the-coin-2ace7ab62b98)
- [React 初窥：JSX 详解](https://github.com/wxyyxc1992/Web-Development-And-Engineering-Practices/blob/master/Refer/React-And-FrontEnd-Engineering/React%20%E5%88%9D%E7%AA%A5/JSX.md)
- [You’re Missing the Point of React](https://medium.com/@dan_abramov/youre-missing-the-point-of-react-a20e34a51e1a)
- [React 是如何重新定义前端开发的](https://zhuanlan.zhihu.com/p/27437770)
- [React JS and why it's awesome](https://www.slideshare.net/AndrewHull/react-js-and-why-its-awesome)
- [Navigating the React.JS Ecosystem](https://www.toptal.com/react/navigating-the-react-ecosystem)
- [React组件的生命周期](https://segmentfault.com/a/1190000006807631)
- [ReactJS组件间沟通的一些方法](http://www.alloyteam.com/2016/01/some-methods-of-reactjs-communication-between-components/)
- [React Router 使用教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html)
- [前后端分离系列文章](http://bbear.me/tag/qian-hou-duan-fen-chi/)
- [ES2015实战——面向未来编程](http://yanhaijing.com/javascript/2016/04/27/es2015-practice/)
- [React 相关经验及发展动态](https://zhuanlan.zhihu.com/purerender)
- [高性能 React 组件](http://taobaofed.org/blog/2016/08/12/optimized-react-components/)
- [React入门与进阶之路由](http://blog.codingplayboy.com/2016/10/24/react_router/)
- [3 Reasons why I stopped using React.setState](https://medium.com/@mweststrate/3-reasons-why-i-stopped-using-react-setstate-ab73fc67a42e)
- [React Higher Order Components in depth](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e)
- [Function as Child Components](https://medium.com/merrickchristensen/function-as-child-components-5f3920a9ace9)
- [Function as Child Components Are an Anti-Pattern](http://americanexpress.io/faccs-are-an-antipattern/)
- [Functional setState of React](https://medium.freecodecamp.org/functional-setstate-is-the-future-of-react-374f30401b6b)
- [Clean Code vs. Dirty Code: React Best Practices](http://americanexpress.io/clean-code-dirty-code/)
- [7 architectural attributes of a reliable React component](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/)
- [All About React Router 4](https://css-tricks.com/react-router-4/)
- [A compilation of React Patterns, techniques, tips and tricks](https://vasanthk.gitbooks.io/react-bits/)
- [Mastering React Functional Components with Recompose](https://blog.usejournal.com/mastering-react-functional-components-with-recompose-d4dd6ac98834)
- [Fractal — A react app structure for infinite scale](https://hackernoon.com/fractal-a-react-app-structure-for-infinite-scale-4dab943092af)
- [React as a UI Runtime](https://overreacted.io/react-as-a-ui-runtime/)

## 范例代码

- [webpack-react-boilerplate](https://github.com/J-F-Liu/webpack-react-boilerplate)
- [parcel-react-boilerplate](https://github.com/J-F-Liu/parcel-react-boilerplate)

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

React introduced a way to write the UI as a function of its state.

view = render(state)

React keeps the application UI in sync with the React state.
Every re-render in React starts with a state change.
When a component re-renders, it also re-renders all of its descendants.
React doesn't re-render the entire app, only re-render the components which need update.

Each render is a snapshot, React plays a “find the differences” game to figure out what's changed between these two snapshots.

A pure component is one that always produces the same UI when given the same props.
In the real world, many of our components are impure.
When a component re-renders, it tries to re-render all descendants, regardless of whether they're being passed a particular state variable through props or not.
So props have nothing to do with re-renders.

React.memo and React.PureComponent are two tools to allow us to ignore certain re-render requests.
The idea of “memorization” is that React will remember the previous snapshot. If none of the props have changed, React will re-use that stale snapshot.

Why isn't “memorization” the default behaviour?

We tend to overestimate how expensive re-renders are. For simple components, re-renders are lightning quick.
If a component has a bunch of props and not a lot of descendants, it can actually be slower to check if any of the props have changed compared to re-rendering the component.

The React team is actively investigating whether it's possible to “auto-memoize” code during the compile step.

context is sorta like “invisible props”, or maybe “internal props”.


## 组件分类

- 纯展示型的组件，数据进，DOM 出，直观明了
- 交互型组件，典型的例子是对于表单组件的封装和加强，大部分的组件库都是以交互型组件为主，比如说 Element UI，特点是有比较复杂的交互逻辑，但是是比较通用的逻辑，强调组件的复用
- 接入型组件，在 React 场景下的 container component，这种组件会跟数据层的 service 打交道，会包含一些跟服务器或者说数据源打交道的逻辑，container 会把数据向下传递给展示型组件
- 功能型组件，路由的 router-view 组件、transition 组件，本身并不渲染任何内容，是一个逻辑型的东西，作为一种扩展或者是抽象机制存在

## 服务端渲染SSR(Server-side rendering)

单页应用的前端框架，对搜索引擎不友好，SEO 效果差。
SSR的原理是服务器执行前端脚本，将渲染结果直接发给浏览器，从而提升 SEO 效果、加快网页显示。

Can not use window in componentWillMount and render methods.