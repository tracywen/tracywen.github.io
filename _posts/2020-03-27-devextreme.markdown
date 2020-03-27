---
layout: post
title:  "DevExtreme使用分享"
date:   2020-03-27 16:22:41 +0800
categories: DevExtreme
---
<!-- more -->

主要是使用过程中一些需要注意的地方:

1.  控件的dataSource依赖于API的response.

    错误写法：
    假如每次API返回的内容不一样，UI上的dropdown内容不会自动更新：
    ```javascript
    apiService.getDropdownList().then(function (r) {
        $scope.dropdownOptions = {
            dataSource: r.data,
            placeholder: "Select One",
            displayExpr: "label",
            valueExpr: "value",
            bindingOptions: {
                value: "selected"
            }
        }
    });
    ```

    正确写法 1：
    ```javascript
    $scope.dropdownOptions = {
        placeholder: "Select One",
        displayExpr: "label",
        valueExpr: "value",
        bindingOptions: {
            dataSource: 'dropdownList',
            value: "selected"
        }
    }
    
    apiService.getDropdownList().then(function (r) {
        $scope.dropdownList = r.data;
    });
    ```

    正确写法 2：
    ```javascript
    $scope.dropdownOptions = {
        displayExpr: "label",
        valueExpr: "value",
        valueChangeEvent: 'keyup',
        bindingOptions: {
            value: "selected"
        },
        onInitialized: function (e) {
            $scope.instance = e.component;
        }
    }
    
    apiService.getDropdownList().then(_.bind(function (r) {
        $scope.instance.option('dataSource', new DevExpress.data.DataSource({
          store: r.data
        }))
    }, this));
    ```
---
2.  Datagrid 的Custom template中，不要写三目运算符，否则grid底部会出现大片空白.

    错误写法：
    ```html
    <div id="gridContainer" dx-data-grid="dataGridOptions" dx-item-alias="item" class="custom-datagrid">
        <div data-options="dxTemplate:{ name:'actionCellTemplate' }">
            <a ng-click="clickViewForm(item.data.id)" class="cell-template-link">
                {{isEdit ? 'Edit' : 'New'}}
            </a>
        </div>
    </div>
    ```
