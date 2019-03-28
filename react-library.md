## 代码分割
- import()
```
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```
- React Loadable

将动态引入(dynamic import)封装成了一个对 React 友好的 API 来在特定组件下引入代码分割的功能。

```
import Loadable from 'react-loadable';

const LoadableOtherComponent = Loadable({
  loader: () => import('./OtherComponent'),
  loading: () => <div>Loading...</div>,
});

const MyComponent = () => (
  <LoadableOtherComponent/>
);
```
- 基于路由的代码分隔

```
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Loadable from 'react-loadable';

const Loading = () => <div>Loading...</div>;

const Home = Loadable({
  loader: () => import('./routes/Home'),
  loading: Loading,
});

const About = Loadable({
  loader: () => import('./routes/About'),
  loading: Loading,
});

const App = () => (
  <Router>
    <Switch>
      <Route exact path="/" component={Home}/>
      <Route path="/about" component={About}/>
    </Switch>
  </Router>
);
```
## 生命周期
[react 生命周期](https://blog.csdn.net/qq_34134278/article/details/81328464)
- 装载
1. constructor
2. static getDerivedStateFromProps()
3. render
4. componentDidMount
- 更新
1. static getDerivedStateFromProps()
2. shouldComponentUpdate()
3. render()
4. getSnapshotBeforeUpdate()
5. componentDidUpdate()
- 卸载
1. componentWillUnmount()
- 报错
1. static getDerivedStateFromError()
2. componentDidCatch()
