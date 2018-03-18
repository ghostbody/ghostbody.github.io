# AngularJS源代码阅读(2) 项目结构

<img src="/posts/front-end/angular/01-angularjs.png">

## 基本信息

- 项目地址：https://github.com/angular/angular.js
- 最新版本：1.6.9

## 目录结构

```
├─benchmarks                # 压力测试指标相关
├─css
├─docs                      # 文档
├─i18n                      # i18n相关
├─logs
├─scripts                   # 项目脚本
├─test                      # 测试相关
├─src                       # 源代码
│  │  .eslintrc.json        # eslint 代码风格控制
│  │  angular.bind.js       # bind jqlite
│  │  Angular.js            # window.angular 的用户接口
│  │  angular.prefix
│  │  angular.suffix
│  │  AngularPublic.js      # 将angular的接口暴露
│  │  apis.js
│  │  jqLite.js
│  │  loader.js             # angular.module在这里定义，函数是setupModuleLoader
│  │  loader.prefix
│  │  loader.suffix
│  │  minErr.js             # 错误处理，在angularjs出错时候可以提供丰富的错误信息
│  │  module.prefix
│  │  module.suffix
│  │  publishExternalApis.js
│  │  shallowCopy.js
│  │  stringify.js
│  │
│  ├─auto
│  │      injector.js       # 核心，angular依赖注入管理，provider
│  │
│  ├─ng
│  │  │  anchorScroll.js
│  │  │  animate.js
│  │  │  animateCss.js
│  │  │  animateRunner.js
│  │  │  browser.js
│  │  │  cacheFactory.js
│  │  │  compile.js             # 核心，compile实现
│  │  │  controller.js          # 核心，controller实现，虽然代码非常短
│  │  │  cookieReader.js
│  │  │  document.js
│  │  │  exceptionHandler.js
│  │  │  filter.js
│  │  │  forceReflow.js
│  │  │  http.js
│  │  │  httpBackend.js
│  │  │  interpolate.js
│  │  │  interval.js
│  │  │  jsonpCallbacks.js
│  │  │  locale.js
│  │  │  location.js
│  │  │  log.js
│  │  │  parse.js
│  │  │  q.js                   # 核心，angular异步库
│  │  │  raf.js
│  │  │  rootElement.js
│  │  │  rootScope.js           # 核心，Scope实现
│  │  │  sanitizeUri.js
│  │  │  sce.js
│  │  │  sniffer.js
│  │  │  templateRequest.js
│  │  │  testability.js
│  │  │  timeout.js
│  │  │  urlUtils.js
│  │  │  window.js
│  │  │
│  │  ├─directive
│  │  │      a.js
│  │  │      attrs.js
│  │  │      directives.js
│  │  │      form.js
│  │  │      input.js
│  │  │      ngBind.js
│  │  │      ngChange.js
│  │  │      ngClass.js
│  │  │      ngCloak.js
│  │  │      ngController.js        # 核心，常用angular指令
│  │  │      ngCsp.js
│  │  │      ngEventDirs.js
│  │  │      ngIf.js                # 核心，常用anuglar指令
│  │  │      ngInclude.js
│  │  │      ngInit.js
│  │  │      ngList.js
│  │  │      ngModel.js             # 核心，常用angular指令，双向数据绑定
│  │  │      ngModelOptions.js
│  │  │      ngNonBindable.js
│  │  │      ngOptions.js
│  │  │      ngPluralize.js
│  │  │      ngRepeat.js            # 核心，常用anuglar指令
│  │  │      ngShowHide.js
│  │  │      ngStyle.js
│  │  │      ngSwitch.js
│  │  │      ngTransclude.js
│  │  │      script.js
│  │  │      select.js
│  │  │      validators.js
│  │  │
│  │  └─filter
│  │          filter.js
│  │          filters.js
│  │          limitTo.js
│  │          orderBy.js
│  │
│  ├─ngAnimate
│  │      .eslintrc.json
│  │      animateChildrenDirective.js
│  │      animateCss.js
│  │      animateCssDriver.js
│  │      animateJs.js
│  │      animateJsDriver.js
│  │      animateQueue.js
│  │      animation.js
│  │      module.js
│  │      ngAnimateSwap.js
│  │      rafScheduler.js
│  │      shared.js
│  │
│  ├─ngAria
│  │      aria.js
│  │
│  ├─ngComponentRouter
│  │      Router.js
│  │
│  ├─ngCookies
│  │      cookies.js
│  │      cookieWriter.js
│  │
│  ├─ngLocale
│  │
│  ├─ngMessageFormat
│  │      .eslintrc.json
│  │      messageFormatCommon.js
│  │      messageFormatInterpolationParts.js
│  │      messageFormatParser.js
│  │      messageFormatSelector.js
│  │      messageFormatService.js
│  │
│  ├─ngMessages
│  │      messages.js
│  │
│  ├─ngMock
│  │      .eslintrc.json
│  │      angular-mocks.js
│  │      browserTrigger.js
│  │
│  ├─ngParseExt
│  │      module.js
│  │      ucd.js
│  │
│  ├─ngResource
│  │      resource.js
│  │
│  ├─ngRoute
│  │  │  .eslintrc.json
│  │  │  route.js
│  │  │  routeParams.js
│  │  │
│  │  └─directive
│  │          ngView.js
│  │
│  ├─ngSanitize
│  │  │  .eslintrc.json
│  │  │  sanitize.js
│  │  │
│  │  └─filter
│  │          linky.js
│  │
│  └─ngTouch
│      │  .eslintrc.json
│      │  swipe.js
│      │  touch.js
│      │
│      └─directive
│              ngSwipe.js
│
```