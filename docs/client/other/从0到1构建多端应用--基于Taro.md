## 从0到1构建多端应用--基于Taro

[TOC]

### 一、环境搭建

```shell
# 安装node.js 并确认
$ node -v
# npm 确认
$ npm -v
# yarn 安装
$ npm install -g yarn
# CLI工具安装
$ yarn global add @tarojs/cli
# 使用taro命令在当前目录下初始化项目
$ taro init myApp
```

### 二、环境运行

```shell
# 将项目导入idea

# 首先需要进行多端开发的配置修改
# 打开config/index.js文件
# 修改outputRoot值为'dist/' + process.env.TARO_ENV

# 打开package.json文件
# 点击左边三角运行所需项目，如dev:weapp、dev:alipay、dev:h5等

# 打开不同端的开发工具导入编译后生成的dist目录下对应端的文件夹，即可在开发工具下实时预览项目
```

三、项目目录

```
├── dist                                编译结果目录
|   ├── alipay                          支付宝小程序编译结果目录
|   └── weapp                           微信小程序编译结果目录
├── config                              配置目录
|   ├── dev.js                          开发时配置
|   ├── index.js                        默认配置
|   └── prod.js                         打包时配置
├── src                                 源码目录
|   ├── assets                          公共资源目录（内含图标等资源）
|   ├── components                      公共组件目录
|   |   └── Btn                         公共组件 Btn 目录
|   |       ├── Btn.js                  公共组件 Btn 逻辑
|   |       └── Btn.scss                公共组件 Btn 样式
|   ├── pages                           页面文件目录
|   |   └── index                       index 页面目录
|   |       ├── components              index 页面的独有组件目录
|   |       |   └── Banner              index 页面的 Banner 组件目录
|   |       |       ├── Banner.js       index 页面的 Banner 组件逻辑
|   |       |       └── Banner.scss     index 页面的 Banner 组件样式
|   |       ├── index.js                index 页面逻辑
|   |       └── index.scss              index 页面样式
|   ├── subpackages                     分包目录（项目过大时建议分包）
|   |   └── profile                     一个叫 profile 的分包目录
|   |       └── pages                   该分包的页面文件目录
|   |           └── index               该分包的页面 index 目录（其下结构与主包的页面文件一致）
|   ├── utils                           项目辅助类工具目录
|   |   └── api.js                      比如辅助类 api 等
|   ├── app.css                         项目总通用样式
|   └── app.js                          项目入口文件
├── package.json                        
└── package.config.json
```



| 序号 | 标题         | 描述                                                        |
| ---- | ------------ | ----------------------------------------------------------- |
| 1    | 公共导入     | 必先导入 import Taro, { Component } from '@tarojs/taro'     |
| 2    | 组件内的标签 | 在Taro之后导入                                              |
| 3    | 资源路径     | 避免使用 @ ，可以使用相对路径                               |
| 4    | require 规范 | js 可以不加后缀，可以是直接路径                             |
| 5    | 静态图片     | 在使用前import保存                                          |
| 6    | 组件公共语法 | class Totale extends Component{  }                          |
| 7    | 页面的标题   | 公共语法内添加 config = {navigationBarTitleText: '标题'}    |
| 8    | 如果有state  | 定义在constructor () {super(...arguments)this.state = {}}中 |
| 9    | 生命周期     | 钩子函数非必须，自定义函数名最好特殊点，必须加render函数    |
| 10   | 定义类名     | 使用className                                               |

