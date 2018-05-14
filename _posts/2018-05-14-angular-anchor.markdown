---
layout: post
title:  "Angular1中的锚点定位"
date:   2018-05-14 16:22:41 +0800
categories: AngularJs
---
<!-- more -->

***$anchorScroll***提供以下两种用法:

- 跳转到元素相关联的指定的hash处
- 设置垂直滚动偏移量

> #### 跳转到元素相关联的指定的hash处

用于跳转到具体的某一个元素位置处，比如在页面底部点击保存按钮时，如果有错误信息，则需要将页面滚动到出错位置处。示例如下：

In html file
```html
<div id="scrollArea" ng-controller="ScrollController">
  <input id="username" type="text" name="username" ng-model="vm.username">
  <a ng-click="save()">Save</a>
</div>
```

In javascript file
```java
angular.module('anchorScrollExample', []).controller('ScrollController', ['$scope', '$location', '$anchorScroll',
  function($scope, $location, $anchorScroll) {
    $scope.save = function() {
      // set the location.hash to the id of
      // the element you wish to scroll to.
      $location.hash('username');

      // call $anchorScroll()
      $anchorScroll();
    };
  }]);
```

```json
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

> #### 设置垂直滚动偏移量

如果设置了这个值，将会指定一个垂直的滚动的偏移量。这种场景经常用于在页面顶部有固定定位的元素, 如导航条，头部等（让出头部空间）

```java
angular.module('anchorScrollOffsetExample', []).run(['$anchorScroll', function($anchorScroll) {
  $anchorScroll.yOffset = 50;   // 总是滚动额外的50像素
}])
```
