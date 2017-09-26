# React技术生态和服务端渲染

> @auth 吴家荣 https://github.com/wujr5

> 这篇文章是我在Matrix做内部分享的具体内容。分享时间：2016年12月24日 上午10:30，地点：中山大学数据科学与计算机学院 A319。分享PPT：[React技术生态和服务端渲染](https://wujiarong.com/sliders/2016-12-24.html)。此文章是倾向于讲稿的，因此口语化可能比较严重。

## A hello world of react

在开始之前，我们先对React有一个直观的认识。

首先明确两点：

* React是一个View层的库
* Browser端的内容，万变不离其宗，最终都要变成JS，CSS，HTML在浏览器端被解析或执行

下面的代码是演示程序的基本结构。

```html
<!DOCTYPE html>

<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello World</title>
    <script src="https://unpkg.com/react@latest/dist/react.js"></script>
    <script src="https://unpkg.com/react-dom@latest/dist/react-dom.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
  </head>

  <body>
    <div id="root"></div>
    <script type="text/babel">
      <!-- Your babel react code goes here -->
    </script>
    <script type="text/javascript">
      // Your javascript code goes here
    </script>
  </body>
</html>
```

**JavaScript写法**

> `JavaScript写法`这个说法可能不准确，先姑且这样说。大概意思是不经过预处理的JS代码。

```js
ReactDom.render(
  React.createElement('h1', null, 'Hello World!'),
  document.getElementById('root')
);
```

**Babel+jsx写法**

```babel
ReactDom.render(
  <h1>Hello World!</h1>,
  document.getElementById('root')
);
```

**效果**

<iframe height='265' scrolling='no' title='ZpvBNJ' src='//codepen.io/gaearon/embed/ZpvBNJ/?height=265&theme-id=0&default-tab=result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='http://codepen.io/gaearon/pen/ZpvBNJ/'>ZpvBNJ</a> by Dan Abramov (<a href='http://codepen.io/gaearon'>@gaearon</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>

上面两种写法的效果是一样的，区别就是后者需要经过Babel进行预处理，把`JSX`代码转化成能在浏览器端运行的`JS`代码，代码放置的地方也不一样，前者放在`<script type="text/babel"></script>`内，后者放在`<script type="text/javascript"></script>`内。

React是前端View层的库，它可以很容易地跟其他`MV*`框架结合使用，本身不涉及数据管理、状态管理等。从上面的代码中可以看到一个大概，它的做法是，生成一个`React Dom`，也就是虚拟DOM，最后把生成的真正的DOM插入到指定的DOM Tree节点中。

这里解析一下虚拟DOM。实现了虚拟DOM的库或架构，可能有不同的实现方案，但大概思路是：通过 [element creation](https://github.com/Matt-Esch/virtual-dom#element-creation) 操作，构建虚拟DOM树`（virtual dom tree）`，然后把虚拟DOM树`render`成真正的DOM，最后把它插入到`DOM Tree`的某个节点中。虚拟DOM技术，还会记录虚拟DOM的状态，当由于某些事件或操作而导致状态发生改变的时候，虚拟DOM会通过 [diff computation](https://github.com/Matt-Esch/virtual-dom#diff-computation) 操作高效低计算出发生改变的地方，然后通过 [patch operations](https://github.com/Matt-Esch/virtual-dom#patch-operations) 操作来应用改变，最小代价地触发浏览器的重绘或重排。

## JSX

初学者对于上面的`Babel + JSX`写法可能有点疑惑。`JSX`可以理解为某种意义上的文本，允许你在`JS`代码里面写携带`HTML`标签的代码，然而这些标签以及标签内的元素既不是字符串也不是`HTML`。

原生的EcmaScript是不支持`JSX`的，因此真正应用`JSX`之前，需要预处理一下，转化成为等效的JS代码。在这里，充当预处理器作用的是`Babel`。很容易想到，`Babel`在处理`JSX`的时候，其实是将它转化成使用`JS`相关的用于DOM的创建或操作的代码。

## React DOM vs. DOM

不可否认，React的火热一定程度上得益于大厂效应，但React确实有过人之处。其中最引人瞩目的一点是，React DOM（也就是React方式的虚拟DOM）与原生DOM的比较。

虚拟DOM的创建和渲染，都是在前端完成的，运行在真正的DOM在浏览器产生和呈现之前，这个时间是`React的运行时`。相对于直接的HTML文本代码，React多了一个虚拟DOM生成和渲染的时间。这个时间，在不同机器的不同浏览器上，都有差异。除此以外，由于在内存里面存储虚拟DOM的内容、状态等数据，React的内存占用量也会比DOM要大。

对于前端来说，浏览器响应用户事件后，非常昂贵的操作是，浏览器的重绘和重排。

* 重绘：重绘是一个元素外观的改变所触发的浏览器行为，例如改变visibility、outline、背景色等属性。浏览器会根据元素的新属性重新绘制，使元素呈现新的外观。重绘不会带来重新布局，并不一定伴随重排。
* 重排：当DOM的变化影响了元素的几何属性（宽或高），浏览器需要重新计算元素的几何属性，同样其他元素的几何属性和位置也会因此受到影响。浏览器会使渲染树中受到影响的部分失效，并重新构造渲染树。这个过程称为重排。每次重排，必然会导致重绘。
  > 导致重排的情况：
  ```
  1. 添加或者删除可见的DOM元素
  2. 元素位置改变
  3. 元素尺寸改变
  4. 元素内容改变（例如：一个文本被另一个不同尺寸的图片替代）
  5. 页面渲染初始化（这个无法避免）
  7. 浏览器窗口尺寸改变
  ```

很明显，相对于渲染时间和内存占用，提高前端性能更为核心的任务是，尽可能地减少重绘和重排的次数。React的虚拟DOM技术和各个生命周期及其钩子函数正是用来解决上述问题的。

## React组件的生命周期

下面是React组件的常用的写法，类内的函数都是React的钩子函数，每个函数都对应React组件一个生命周期。

```js
import react, { Component } from 'react';

class ExampleComponent extends Component {

  // Mounting

  constructor() {
    // do something initialized, set default props or initial state
  }

  componentWillMount() {
    // do something before component mounting
  }

  render() {
    // return the jsx code, it will be the component's dom content, if state changes, it will be rerun
  }

  componentDidMount() {
    // do something after component mounting
  }

  // Updating

  componentWillRecieveProps() {
    // do something before a mounted component receives new props
  }

  shouldComponentUpdate() {
    // do something before rendering when new props or state are being received
  }

  componentWillUpdate() {
    // do something before rendering when new props or state are being received
  }

  componentDidUpdate() {
    // do something after updating occurs
  }

  // Unmounting

  componentWillUnmount() {
    // do something before a component is unmounted and destroyed
  }

}
```

**关键点**

* 组件的`props`，`state`非常重要，组件的动态内容主要依赖这两个属性
* 改变`props`的方式：改变父级元素传递的`props`、直接改变`this.prpos`
* 改变`state`的方式：通过组件的`setState`函数改变、
* 组件的`state`变化，会引起组件的`rerender`
* 组件有多种组合方式，同级组合、父子组合等
* 组件在不同的生命周期，可以触发不同的钩子函数

通过灵活使用组合方式、组件的生命周期函数，借助React高效的更新和渲染方式，可以在前端做动态性、表现力都很强的单页面应用、多页面应用、H5应用等。

## A game demo of react

我们来看一个使用React来做的俄罗斯方块游戏的Demo。

[github: 用React、Redux、Immutable做俄罗斯方块](https://github.com/chvin/react-tetris)

[Demo](https://chvin.github.io/react-tetris/)

关键技术：React, Redux, Immutable, Webpack, Babel, Web Audio

## React生态

由于React是前端View层的库，使用方式十分灵活，可以跟现有的很多库或框架结合使用。也因此衍生了React的技术生态。我们可以通过这个页面一窥究竟：[Complementary Tools](https://github.com/facebook/react/wiki/Complementary-Tools)

![](http://ww1.sinaimg.cn/large/ed796d65gw1fb1lfmh6wxj210a9k6e85.jpg)

我所能想到的React的使用方式有：

* 传统页面应用 or MV*：react + others library or framework
* SPA：react + react-router
* SPA + flux：react + react-router + flux库（Redux等）
* React Native开发跨平台移动应用
* Universal App：Server Side Rendering

### React生态：react-router

![](https://github.com/ReactTraining/react-router/raw/master/logo/vertical@2x.png)

react的前端路由，可以结合redux使用，是利用React做SPA的不可或缺的技术。实现方式：

* 基于HTML5 history API
* 基于前端`#字符串`

与其他SPA框架实现思路类似。

### React生态：flux

几个关键点：

* facebook提出的flux更多是一种设计模式，而不是框架
* flux有多重实现方案，redux是其中之一
* 思路：单向数据流
* 关键点：`dispatcher`，`store`，`action`，`reducer`，`view`
* `store`可以管理react组件的状态，当`store`改变的时候，会最低代价地更新react组件

> Flux is the application architecture that Facebook uses for building client-side web applications. It complements React's composable view components by utilizing a unidirectional data flow. It's more of a pattern rather than a formal framework, and you can start using Flux immediately without a lot of new code.

> *From [Flux: In Depth Overview](https://facebook.github.io/flux/docs/in-depth-overview.html)*

关键思想：

![](https://github.com/facebook/flux/raw/master/docs/img/flux-diagram-white-background.png)

**Redux**

> Redux is a predictable state container for JavaScript apps.

Redux是一个可预测的状态容器，是flux思想的一种实现方案。

> Redux 的灵感来源于 Flux 的几个重要特性。和 Flux 一样，Redux 规定，将模型的更新逻辑全部集中于一个特定的层（Flux 里的 store，Redux 里的 reducer）。Flux 和 Redux 都不允许程序直接修改数据，而是用一个叫作 “action” 的普通对象来对更改进行描述。

> 而不同于 Flux ，Redux 并没有 dispatcher 的概念。原因是它依赖纯函数来替代事件处理器。纯函数构建简单，也不需额外的实体来管理它们。你可以将这点看作这两个框架的差异或细节实现，取决于你怎么看 Flux。Flux 常常被表述为 (state, action) => state。从这个意义上说，Redux 无疑是 Flux 架构的实现，且得益于纯函数而更为简单。

> 和 Flux 的另一个重要区别，是 Redux 设想你永远不会变动你的数据。你可以很好地使用普通对象和数组来管理 state ，而不是在多个 reducer 里变动数据。正确且简便的方式是，你应该在 reducer 中返回一个新对象来更新 state， 同时配合 object spread 运算符提案 或一些库，如 Immutable。

> 虽然出于性能方面的考虑，写不纯的 reducer 来变动数据在技术上是可行的，但我们并不鼓励这么做。不纯的 reducer 会使一些开发特性，如时间旅行、记录/回放或热加载不可实现。此外，在大部分实际应用中，这种数据不可变动的特性并不会带来性能问题，就像 Om 所表现的，即使对象分配失败，仍可以防止昂贵的重渲染和重计算。而得益于 reducer 的纯度，应用内的变化更是一目了然。

### React生态：UI组件库

UI有很多组件库，这里是官方列举的组件库资源：（更新于2016/12/24）

* **[Color Picker](https://mobiscroll.com/color-picker)** Color picker control with single and multiple select
* **[Responsive Calendar](https://mobiscroll.com/responsive-calendar)** Responsive Calendar for mobile, tablet and desktop
* **[Mobile date & time picker control](https://mobiscroll.com/mobile-date-and-time-picker)** Cross platfrom mobile date & time picker control for React.
* **[Mobiscroll](https://mobiscroll.com)** UI components for mobile web and hybrid apps.
* **[ag-Grid](https://www.ag-grid.com)** Advanced data grid / data table for React.
* **[Grommet](http://grommet.io)** The most advanced open source UX framework for enterprise applications.
* **[React Web](https://github.com/taobaofed/react-web)** A framework for building web apps with React.
* **[React Mdl](https://github.com/tleunen/react-mdl)** React Components for Material Design Lite.
* **[Amaze UI React](https://github.com/amazeui/amazeui-react) (in Chinese):** [Amaze UI](https://github.com/allmobilize/amazeui) components built with React.
* **[Belle](https://github.com/nikgraf/belle/):** Configurable React Components with great UX
* **[Khan Academy's component library](http://khan.github.io/react-components/)**
* **[Elemental UI](http://elemental-ui.com):** A UI toolkit for React websites and apps, themeable and composed of individually packaged components
* **[Halogen](http://madscript.com/halogen):** A collection of highly customizable loading spinner animations with React.
* **[react-bootstrap](https://github.com/stevoland/react-bootstrap):** Bootstrap 3 components built with React.
* **[react-bootstrap-table](https://github.com/AllenFang/react-bootstrap-table):** It's a react table for Bootstrap.
* **[react-bootstrap-dialog](https://github.com/akiroom/react-bootstrap-dialog):** React Dialog component for react-bootstrap, instead of `window.alert` or `window.confirm`
* **[Topcoat UI Components](https://github.com/kjda/react-topui):** Topcoat UI Components built with react.
* **[react-foundation-apps](https://github.com/akiran/react-foundation-apps):** Foundation Apps components built with React
* **[orb](http://nnajm.github.io/orb/):** React pivot grid.
* **[react-topcoat](https://github.com/plaxdan/react-topcoat):** Topcoat components built with React.
* **[react-slick](https://github.com/akiran/react-slick):** Carousel component built with React
* **[react-lorem-component](https://github.com/martinandert/react-lorem-component):** Lorem Ipsum placeholder component.
* **[wingspan-forms](https://github.com/wingspan/wingspan-forms):** React library for dynamic forms & grids; widgets provided by KendoUI.
* **[react-translate-component](https://github.com/martinandert/react-translate-component):** React component for i18n.
* **[newforms](http://newforms.readthedocs.org/en/latest/):** Form-handling library which renders to `React.DOM` components on the client and the server.
* **[react-highlight](https://github.com/akiran/react-highlight):** React component for syntax highlighting
* **[react-responsive-mixin](https://github.com/akiran/react-responsive-mixin):** Mixin to develop responsive react components
* **[react-chosen](https://github.com/chenglou/react-chosen):** React wrapper for Chosen jQuery.
* **[react-select](https://github.com/JedWatson/react-select):** Native React Select / Multiselect input field, similar to Selectize / Chosen / Select2
* **[react-ladda](https://github.com/jsdir/react-ladda):** React wrapper for Ladda buttons.
* **[react-scroll-components](https://github.com/jeroencoumans/react-scroll-components):** A set of components and mixins that react to page scrolling.
* **[react-forms](http://prometheusresearch.github.io/react-forms/):** Form rendering and validation for React
* **[valuelink](https://github.com/Volicon/valuelink):** Full-featured two-way data binding and forms validation with extended React links.
* **[tcomb-form](https://github.com/gcanti/tcomb-form):** Automatically generate form markup, automatic labels and inline validation from a domain model
* **[react-treeview](https://github.com/chenglou/react-treeview):** Easy, light, flexible tree view.
* **[react-multiselect](https://github.com/neodon/react-multiselect):** Filter and select from a list of items.
* **[react-date-picker](https://github.com/Hacker0x01/react-datepicker):** A simple and reusable datepicker component for React.
* **[react-textarea-autosize](https://github.com/andreypopp/react-textarea-autosize):** Like `<textarea />` but resizes automatically to fit all its content.
* **[react-growable-textarea](https://github.com/mschipperheyn/react-growable-textarea):** Resizes `<textarea />` to fit all its content using a css shim (no javascript calculations).
* **[react-input-autosize](https://github.com/JedWatson/react-input-autosize):** Like `<input />` but resizes automatically to fit all its content.
* **[React-TimeAgo](https://www.npmjs.org/package/react-timeago)** A minimal live updating Time Ago component that smartly converts any time to a ‘ago’ or ‘from now’ format and keeps it updated.
* **[react-maps](http://iamdanfox.github.io/react-maps/)** Embed Google Maps. Supports markers, polylines and programmatic mutation.
* **[material-ui](http://material-ui.com)** A set of React Components that implement Google's Material Design.
* **[storybook-addon-material-ui](https://github.com/sm-react/storybook-addon-material-ui)** A storybook addon for material-ui
* **[Ant Design of React](http://github.com/ant-design/ant-design)** An enterprise-class UI design language and React-based implementation.
* **[react-tappable](https://github.com/JedWatson/react-tappable)** A Tappable React Component that provides native-feeling onTap events for mobile React apps
* **[react-tour-guide](https://github.com/jakemmarsh/react-tour-guide)**A ReactJS mixin to give new users a popup-based tour of your application
* **[react-contentEditable](https://github.com/liamzebedee/reactjs-contenteditable)** Example component for contentEditable elements
* **[react-dnd](https://github.com/gaearon/react-dnd)** Flexible HTML5 drag-and-drop mixin for React with full DOM control
* **[react-autosuggest](https://github.com/moroshko/react-autosuggest)** WAI-ARIA compliant React autosuggest component
* **[react-document-title](https://github.com/gaearon/react-document-title)** Declarative, nested, stateful, isomorphic document.title for React
* **[react-image](https://github.com/yuanyan/react-image):** Like `<img />` and Enhanced Image Component for React.
* **[react-intense](https://github.com/brycedorn/react-intense):** A component for viewing large images up close
* **[react-mq](https://github.com/yuanyan/react-mq):** Easy Media Query Helper for React.
* **[react-timesheet](https://github.com/yuanyan/react-timesheet):** Visualize your data and events with sexy time sheet component.
* **[react-switch-button](https://github.com/gfazioli/react-switch-button):** Beautiful React Switch button component
* **[react-amiga-guru-meditation](https://github.com/gfazioli/react-amiga-guru-meditation):** React JS Class to display a Amiga Guru Meditation Tribute
* **[react-notification](https://github.com/pburtchaell/react-notification):** Snackbar style notifications
* **[react-swipe-views](https://github.com/damusnet/react-swipe-views):** React Swipe Views
* **[react-spreadsheet](https://github.com/felixrieseberg/React-Spreadsheet-Component):** React Spreadsheets / Editable tables with Excel-Style keyboard input and navigation
* **[react-dropzone](https://github.com/felixrieseberg/React-Dropzone):** React Dropzone for File-Uploads
* **[react-twitter-typeahead](https://github.com/erikschlegel/React-Twitter-Typeahead):** React auto-suggestion component
* **[react-sparklines](https://borisyankov.github.io/react-sparklines/):** Beautiful and expressive sparklines component
* **[Winterfell](https://github.com/andrewhathaway/Winterfell):** Generate complex, validated and extendable JSON-based forms in React
* **[react-selectize](https://furqanzafar.github.io/react-selectize/):** A stateless & flexible Select component, designed as a drop in replacement for React.DOM.Select, inspired by Selectize 
* **[react-joyride](https://github.com/gilbarbara/react-joyride):** Create walkthroughs and guided tours for your ReactJS apps.
* **[react-stamp](https://github.com/stampit-org/react-stamp):** Composables for React.
* **[mobservable-react-buttons](https://github.com/dschalk/mobservable-react-buttons):** These are rollover buttons with some extra functionality. A more elaborate demo is running at [http://schalk.ninja](http://schalk.ninja) but I don't presently have a repo available.
* **[UXCore](http://uxco.re/components/calendar/) (in Chinese):** [UXCore](http://uxco.re/components/calendar/) UXCore components built with React.
* **[reactstrap](https://reactstrap.github.io/):** Simple Bootstrap 4 components built with [tether](http://tether.io/)
* **[Onsen UI React Components](https://onsen.io/v2/react.html):** UI components for hybrid mobile apps with both Material Design for Android and flat design for iOS.
* **[react-pagenav](https://github.com/zxdong262/react-pagenav):** react pagenav component. [[demo](http://html5beta.com/apps/react-pagenav.html)]
* **[React Amazing Grid](https://github.com/Amazing-Space-Invader/react-amazing-grid)** Flex grid with inline styles.
* **[Selectivity](https://arendjr.github.io/selectivity/):** Modular and light-weight selection library.
* **[react-jogwheel](https://github.com/marionebl/react-jogwheel)**: Take control of your CSS keyframe animations with React
* **[Wijmo React Interop](http://wijmo.com/blog/how-to-use-wijmo-controls-in-reactjs-apps/)**: How to use Wijmo controls in ReactJS apps.
* **[Wijmo FlexGrid for React](http://demos.wijmo.com/5/React/FlexGridIntro/FlexGridIntro/)**: A high-performance datagrid that is small and extensible with ReactJS support.
* **[Wijmo FlexChart for React](http://demos.wijmo.com/5/React/FlexChartIntro/FlexChartIntro/)**: A full-featured chart control with ReactJS support.
* **[Wijmo Input controls for React](http://demos.wijmo.com/5/React/InputIntro/InputIntro/)**: A set of form input controls (AutoComplete, InputDate, InputNumber, InputMask, etc) with ReactJS support.
* **[Wijmo Gauges for React](http://demos.wijmo.com/5/React/GaugeIntro/GaugeIntro/)**: Linear and radial gauges and a bullet graph with ReactJS support.
* **[Semantic-ui](http://react.semantic-ui.com/)**: The official Semantic-UI-React integration, Advanced components and declarative UI.
* **[markdown-to-jsx](https://www.npmjs.com/package/markdown-to-jsx)**: compiles markdown into safe React JSX without using dangerous escape hatches
* **[chartify](https://github.com/kirillstepkin/chartify)**: Ultra lightweight and customizable React.js chart component
* **[react-sigma](https://www.npmjs.com/package/react-sigma)**: Lightweight but powerful library for drawing network graphs

### React生态：React Native

[React Native](https://facebook.github.io/react-native/) 是一个可以让你仅仅使用JS就能写跨平台移动应用的库。

写法与写React非常类似。同时可以结合Native的代码一起构建Native Application。

```babel
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class WhyReactNativeIsSoGreat extends Component {
  render() {
    return (
      <View>
        <Text>
          If you like React on the web, you'll like React Native.
        </Text>
        <Text>
          You just use native components like 'View' and 'Text',
          instead of web components like 'div' and 'span'.
        </Text>
      </View>
    );
  }
}
```

```babel
import React, { Component } from 'react';
import { Image, ScrollView, Text } from 'react-native';

class AwkwardScrollingImageWithText extends Component {
  render() {
    return (
      <ScrollView>
        <Image source={{uri: 'https://i.chzbgr.com/full/7345954048/h7E2C65F9/'}} />
        <Text>
          On iOS, a React Native ScrollView uses a native UIScrollView.
          On Android, it uses a native ScrollView.

          On iOS, a React Native Image uses a native UIImageView.
          On Android, it uses a native ImageView.

          React Native wraps the fundamental native components, giving you
          the performance of a native app, plus the clean design of React.
        </Text>
      </ScrollView>
    );
  }
}
```

## 服务端渲染

前端框架的运行时是在前端进行的。也就是说从页面加载完成，到用户看到完整的页面，中间有一个执行过程，而这个执行时间的长短，跟应用的复杂度有关，也跟框架的实现方式有关。React的渲染时间与其他框架的渲染时间，对比如下：

![](https://camo.githubusercontent.com/098f7ef99f6121e377868dbee21d95f657e3e8f8/687474703a2f2f6176616c6f6e6a732e636f64696e672e6d652f7374796c65732f706572666f726d616e63652e6a7067)

> from [avalon](https://github.com/RubyLouvre/avalon)

直观的体验就是，如果渲染时间过长，会有一种白屏或卡顿的感觉。

服务端渲染，就是这个问题的一种解决方案。我们再看一下前端渲染存在的其他问题。

### 前端渲染存在的问题

![](http://nerds.airbnb.com/wp-content/uploads/2013/11/isomorphic-client-side-mvc.png)

* SEO问题
* 性能问题（加载时间、打包大小、解析渲染等）

### Universal app

> **Isomorphism** is the functional aspect of seamlessly switching between client- and server-side rendering without losing state. **Universal** is a term used to emphasize the fact that a particular piece of JavaScript code is able to run in multiple environments.

Isomorphism: not losing state, rendering everywhere

Universal: no duplication, example: `Lodash`, uesed everywhere

### Server Side Rendering

**React的做法**

要思考的问题：

1. 路由问题
2. 状态问题
3. 数据预请求问题

react的做法：

```js
import { renderToString } from 'react-dom/server';
```

react + react-router + redux的做法：

```babel
import { renderToString } from 'react-dom/server';

// a peice of code
renderToString(
  <Provider store={ store }>
    <RouterContext { ...props } />
  </Provider>
);

// a peice of code
match({ routes, location }, (error, redirectLocation, renderProps) => {
  if (error) {
    res.status(500).send(error.message);
  } else if (redirectLocation) {
    res.status(302).redirect(redirectLocation.pathname + redirectLocation.search);
  } else if (renderProps) {
    serverSideRender(
      renderProps,
      pugText,
      store
    ).then(function (html) {
      res.status(200).send(html);
    }, function (error2) {
      res.status(500).send(JSON.stringify(error2));
    });
  } else {
    res.status(404).send();
  }
});
```

## Reference

* [HTML Presentation Framework: Reveal.js](https://github.com/hakimel/reveal.js/)
* [Comparing React.js performance vs. native DOM](https://objectpartners.com/2015/11/19/comparing-react-js-performance-vs-native-dom/)
* [高性能JavaScript 重排与重绘](http://web.jobbole.com/83164/)
* [浏览器的重绘与重排](http://kb.cnblogs.com/page/169820/)
* [Virtual DOM and diffing algorithm](https://gist.github.com/Raynos/8414846)
* [virtual-dom](https://github.com/Matt-Esch/virtual-dom)
* [Isomorphic JavaScript: The Future of Web Apps](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/)
* [Isomorphism vs Universal JavaScript](https://medium.com/@ghengeveld/isomorphism-vs-universal-javascript-4b47fb481beb#.wkh0bwcze)
