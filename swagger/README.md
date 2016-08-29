# Swagger API 管理

## 概述
Swagger 2.0 规范声明了对于一个项目的 API 应该单独由一个 Swagger yaml/json 文件来维护
。这在某种程度上增加了文档管理的难度。

简化管理的思想主要是利用 json和yaml 的引用规则，将一个大的 Swagger 文件分散成许多小的文件。
通过一些简单的约定来大大提高 Swagger API 文档维护和扩展的便捷性。

参考资料: http://azimi.me/2015/07/16/split-swagger-into-smaller-files.html

## 使用方法
简单的示例，如何将 Defination 分离:

``` yaml
swagger: '2.0'
info:
  ...
paths:
  ...
definitions:
  $ref: ./definitions/index.yaml
```
约定 index.yaml 为每个文件夹中的根文件，一般都是通过这个文件来实现间接引用。
具体步骤:
1. 在 definations 文件夹中创建一个 index.yaml 文件, 然后根据需要创建对应的实体对象 Defination1等等
``` yaml 
Defination1:
    $ref: ./Defination1.yaml
Defination2
    $ref: ./Defination2.yaml

```
``` 
目录结构
|-- definations
    |-- index.yaml
    |-- Defination1.yaml
    |-- Defination2.yaml
    |-- ...
```

2. Defination1 等实体类中的内容, 就是正常的编写文档去掉名字，因为名字已经在根目录中引用了。
``` yaml
type: object
description: '仪表盘详情'      
properties:
  id:
    type: string
  name:
    type: string
  dashwindowIdList:
    type: array
    items:
      type: string
  type:
    type: string
```
## 合成文档
`json-refs`, `js-yaml` 这两个模块提供了解析 json/yaml 引用的 api。利用此可以实现文档的拼接，
即将分离的文档重新合成为一个 swagger 文档。然后可以使用 swagger2markup 来进行文档的生成和扩展，或者用UI展示。
使用方法:
1. npm install 安装依赖
2. node resolve.js 生成文档，文档名称可以在js代码中修改。

## todo
Defination 实际上应该也和paths中的处理统一，就是安装不同的模块来分类处理
比如分层 dashboard 和 reference 等来单独存放管理。
由于时间问题这里暂时没有处理，提供一个思路，先讨论一下是否可行。。。

