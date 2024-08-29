---
date: 2024-08-27
title: tableau viz extension开发入门
tags:
  - tableau
  - code
description: 摘要：本教程主要参考官方帮助文档，并用一个实际开发案例展示Tableau 可视化扩展开发的过程。
---

# 准备工作

## 简介

本教程主要参考官方帮助文档 https://tableau.github.io/extensions-api/docs/installation  并用一个实际开发案例展示。

Tableau 可视化扩展(viz extension) API 允许开发人员为 Tableau 创建可视化扩展。Tableau 可视化扩展是可以在仪表板中与 Tableau 交互和通信的网络应用程序。它可以像其他仪表板对象一样放置在仪表板中。可视化扩展创建新的视图类型，用户可以通过工作表标记卡访问。

Tableau 可视化扩展 API 是一个 JavaScript 库，它按照包含与 Tableau 组件通信所需的类和方法的命名空间进行组织。

在Tableau 2024.2 以上版本中，点击标记，选择添加扩展程序，即可选择可视化扩展，调用第三方的 js 代码在 Tableau 中实现可视化。

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NDAyZjdkNDJjZTNmYzIzMDk2OTk0YjQwMTlmMjVkNmNfaW5zemdmTkRMRzVaV2xZVWRheUoxZFlaZ2JsVVJ4dmdfVG9rZW46TDNtZGJOZ1FOb3BjVDh4Qm1QUWN6Nmd3bkJiXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDRjZDczNjYwYTc5ZWE3OGZkMTM4YTk3ZjQ0Y2UxMWJfT3lzMjhDNUtzbEhuUXk2YlA0YW5FVkw3cUk2bFg3UnVfVG9rZW46VEdwRGJJMjFFb2JSQkN4UEtvOGNMVDRibkJmXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

本教程设定是 windows 操作系统环境， macOS 大同小异，意图让用户入门 Tableau可视化扩展的开发， 并用实际案例向用户展示效果。

## 环境搭建

Tableau 扩展包含什么？一个 Tableau 扩展包括一个 XML 文件（ `.trex` ），一个使用 Tableau 提供的 JavaScript 库的网页（ `.html` ），以及包含您扩展逻辑的 JavaScript 文件（ `.js` ）（或文件）。

需要 Node.js 和 npm 来运行可视化扩展示例。Node.js 是一个 JavaScript runtime环境。npm 是 Node.js 的包管理器，当您安装 Node.js 时会自动安装。

Node.js 请安装版本 v20.11.1，macOS 访问以下网址然后用 brew 方式安装https://nodejs.org/en/download/package-manager

在 windows 操作系统下，访问这个地址下载安装即可

https://nodejs.org/en/blog/release/v20.11.0

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=YjhkNTNlM2UyMWViNjY2MTdmNzk1Nzk0YzNiNGM1ZTdfVmliUTA1ZVpnZGtUZWNEeXo5QUxmQ292WmlCRGJvSHlfVG9rZW46QVU4dWJuSGNvb3puZ3h4SG9VcmM4WlJXbnBiXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

代码编辑器推荐使用vscode，https://code.visualstudio.com/download

安装vscode之后打开项目文件夹，然后右键 在集成终端中打开，即可输入后面需要用到的终端命令 npm start 等

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ODgzZjM0OThkNGVjZmEwNzhiNDExZWVmYzA5ZDRmOTRfTHk2WE0wYVdUUjBDZVl5QjRpYThrTnZjR3A5eEc1d2FfVG9rZW46UXRoTGJyNFhzb3VRWlR4cUl5ZmNYcmptbnZXXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

- 获取 API 的源文件可以点击以下链接下载
    

https://github.com/tableau/extensions-api/archive/main.zip

下载完成之后可以看到一个压缩包，箭头所指方向就是官方的示例可视化扩展程序，稍后会介绍

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ODA0OWMxMDZhOGNhMjE1NTViNDA4MTkxMTM3ODU0MTRfNk96WDl4cE1aQXVmbXRPTzNrTnZiMlNwSEhsMHMxNWRfVG9rZW46U0pCTmJ2RjJWb0dlNTZ4UnY0YWN5VGtBbnNiXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

- 或者直接使用我下载并配置好 npm 包环境的文件夹
    

暂时无法在飞书文档外展示此内容

请注意，解压之后这个文件夹如果要运行代码，还需要

1、导航至 `extensions-api-main` 或 `extensions-api` 目录，即根目录

2、安装 npm 的组件，请运行以下 npm 命令来安装和构建包，如果在这一步不成功，请使用我配置好的文件夹，用管理员权限打开压缩包解压，然后直接跳过这一步进入第 3 步

**npm run build**

3、启动服务器，请运行以下 npm 命令：

**npm start**

4、启动命令运行一个脚本，通过端口 `8765` 启动 Web 服务器。您只需要首次安装 Web 服务器组件。之后，只需启动 Web 服务器，使用 npm start 即可。启动命令使用了 npm http-server 包，这是一个简单的 HTTP 服务器，使用 Node.js 为浏览器提供静态文件服务。

5、请打开浏览器窗口，并在菜单栏中访问以下 URL：

`http://localhost:8765/assets/flex.png`

6、如果您的本地 Web 服务器正常运行，您应该能看到一张图片。如果不行，请检查您启动 Web 服务器的命令或终端窗口，并查看是否有错误消息。您应该能看到 HTTP 活动的输出。如果不行，请再次运行 npm 命令。在运行 Web 服务器时，不要关闭这个命令或终端窗口。

npm start 之后效果如图

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NjU0ZTE1YzY3NDRjOTk5ZWMyNmFkMGY0MzU5YTM2NWRfWXZaWjdERGtjTXVWVG9ha3RVNDFMOHlWTFZIRERnQ3RfVG9rZW46R3NKSmIyUXpzb2IwR2x4WlhyNGNtbGNRbjBlXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjM5MDQxOWM1OTJkNzI1ZGI4YzJhNGNmZjU0Y2YyOTVfbTc5bElSTFIzcXFtb2l0ZTR5T1NSY2tTcm5teTJ0UURfVG9rZW46Uk4wMmI1Vmdub1JBcUJ4aFlMd2NpMDlTblpkXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

# 开发可视化扩展

## 官方示例

我们从这个案例开始，看看可视化扩展程序如何运行的，首先打开 Tableau

在 Viz 扩展中，选择添加扩展。

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ODdiNjQ1ZmQ0OTEyZWVkYmY5MmE3YzU1ZjE0ZTA1NGRfNjFmNE5uZUlNb3RBYmM4MG1ReG02bDRmZDlpR2hYdlFfVG9rZW46WkJzMmJTY0FDb2piTkp4SjRud2NhV3hJblllXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

在出现的添加扩展对话框中，选择访问本地扩展。

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NmY5NzAzZDU3YWYyNTFjMjNlMDY4YjI5ZTg4MTE0ZWRfdW5rNkJnaVBkNGlUTEZYbGZtMUt6SXVYNDZrcXRzb0dfVG9rZW46VlE4TWJFUURWb0FZdFp4VmNxMWNnYlVobmlnXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

每个 Tableau 扩展都有一个元文件（.trex），描述了扩展并标识了 Web 应用程序的位置。有关创建 `.trex` 元文件的更多信息，请参阅 Tableau 可视化扩展元文件。

上面的访问本地扩展点击之后，导航至已连接散点图配置文件（ `.trex` ）所在目录: \extensions-api\Samples\Viz\ConnectedScatterplot

打开 `connectedScatterplot.trex` 文件。如果提示，请点击“确定”以允许视图扩展在工作表中运行。该扩展是一个网络启用的扩展，这意味着它能够访问 Tableau 之外的资源，并且不在 Tableau 沙盒中托管。这里的网络是指我们用 npm start 运行的这个服务的网络。

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDQwYTA1ODdlNTE0YzYxNzdkYmJiOGEwZWUyMjBhZmFfY0RzMFFvYTdESFduUVlwQ3JWbWU4NGJLVDd2bDY4R0xfVG9rZW46T0dwMGJjUk9tb3hwN2R4akdIQWNacnRIbm5nXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

在工作表中，将利润字段拖放到 X 编码框中。将“销售”字段拖到 Y 编码框中。将“订单日期”字段拖放到文本编码框中。右键点击蓝色药丸，现在显示为 YEAR(Order Date)，将其改为月。连接散点图在视图中出现。

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=MTE5NGVkYTA0NjFhNGM1MzFiNDE4YzI1YzNmZTg4YjBfUTR6WlpGQUx4YVA2TmZrdFpjSVJqaWxmYmYwR3RKYmtfVG9rZW46SnlTaWJFdWtpb01xVGN4NXRTdGNCbmJVbmhmXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

## 创建“Hello World”可视化扩展

### 创建扩展并使用

接下来我们创建一个 “Hello World” 可视化扩展，来观察最基本的程序如何构成的。我们需要在 extensions-api-main by wenhua\Samples\Viz 这个目录下创建HelloVizExtensions 文件夹，然后在这个文件夹中创建 trex、js、html 三个文件。

HelloVizExtension.trex

trex 文件实质上是一个 XML 文件，描述了扩展，并提供了将扩展与 Tableau 注册的信息。对于此文件内容的描述，请参阅视图表现文件元素。

这些字段的详细含义如下：

对于 `<worksheet-extension id=" ">` ，使用反向域名表示法来唯一标识扩展（ `com.example.extension.hello.demo` ）

对于 `<source-location>` ，请确保此设置指定了您的 Web 应用程序的 URL。您必须使用 HTTPS 协议。此要求的例外是 `localhost` ，在那里您可以使用 HTTP。例如，如果您创建了一个 `HelloVizDemo` 文件夹，并希望在您的计算机上使用端口 8765 本地托管文件，您可能会使用： `http://localhost:8765/Samples/HelloVizDemo/HelloVizExtension.html`

指定运行扩展所需的 Extensions API 库的最小版本的 `<min-api-version>` 元素。

当前，viz 扩展中并未实现 `<icon>` 元素。此元数据用于为扩展提供品牌标识，就像在关于对话框中所显示的那样。

为您的扩展提供 `name` （ `Hello Viz Extensions!` ）。manifest 文件可以本地化，因此请在 `<resources>` 部分的适当 `<text>` 元素中提供名称（或名称）。

在创建了扩展的 HTML 和 JavaScript 文件后，您使用这个 `.trex` 文件将扩展添加到 Tableau 工作表中。要这样做，请打开一个工作表，在“标记”卡上，展开“标记类型”下拉菜单。在“可视化扩展”下，选择“添加扩展”。在出现的“添加扩展”对话框中，选择“访问本地扩展”。浏览到您刚刚创建的 manifest 文件的位置，例如 `HelloVizExtension` 文件夹，并选择 `HelloVizExtensions.trex` 文件。

关于清单文件以及添加版本信息的信息，请参阅 Tableau Viz 扩展清单。

https://tableau.github.io/extensions-api/docs/vizext/trex_viz_manifest

```XML
<?xml version="1.0" encoding="utf-8"?>
<manifest manifest-version="0.1" xmlns="http://www.tableau.com/xml/extension_manifest">
  <worksheet-extension id="com.tableau.extension.demo.hello.viz" extension-version="0.1.0">
    <default-locale>en_US</default-locale>
    <name resource-id="name"/>
    <description>Hello Viz Extension</description>
    <author name="tableau" email="projectfrelard@tableau.com" organization="tableau" website="https://www.tableau.com"/>
    <min-api-version>1.1</min-api-version>
    <source-location>
      <url>http://localhost:8765/Samples/Viz/HelloVizExtension/HelloVizExtension.html</url>
    </source-location>
    <icon/>
    <encoding id="drop">
      <display-name>Drop...</display-name>
    </encoding>
  </worksheet-extension>
  <resources>
    <resource id="name">
      <text locale="en_US">Hello Viz Extension</text>
    </resource>
  </resources>
</manifest>
```

HelloVizExtension.html

在 `HelloVizExtensions` 文件夹（或你存放 `.trex` 文件的地方），创建一个名为 `HelloVizExtension.html` 的文件。

这段代码创建了一个非常简单的页面，显示一行文本。

```HTML
 <!DOCTYPE html>
 <html>
   <head>
    <title>Hello Viz Extensions</title>

    <!-- Tableau Extensions API Library  -->
    <script src="../../../lib/tableau.extensions.1.latest.js"></script>

    <!-- Our extension's code -->
    <script src="./HelloVizExtension.js"></script>
   </head>

   <body>
    <div style="width: 100%; height: 100%; position: fixed" id="content">
       <p>Drag a field on to the tile labeled <b>Drop</b> on the Marks card.</p>
    </div>
   </body>
 </html>
```

HelloVizExtension.js

下一步是创建调用扩展 API 的 JavaScript 代码。在你的 JavaScript 代码中（无论是在 HTML 页面中还是在单独的 JavaScript 文件中），首先需要初始化扩展。为此，你需要调用 `tableau.extensions.initializeAsync()` 。该函数在初始启动操作完成后返回，此时扩展已准备好使用。扩展 API 遵循 CommonJS Promises/A 标准，用于异步方法调用。

```JavaScript

'use strict';

// Wrap everything in an anonymous function to avoid polluting the global namespace
(function () {
  window.onload = tableau.extensions.initializeAsync().then(() => {
    // Get the worksheet that the Viz Extension is running in
    const worksheet = tableau.extensions.worksheetContent.worksheet;
    console.log(worksheet);

    console.log(`The name of the worksheet is ${worksheet.name}`);

    // Listen to event for when the summary data backing the worksheet has changed.
    // This tells us that we should refresh the data and encoding map.
    worksheet.addEventListener(
      tableau.TableauEventType.SummaryDataChanged,
      updateDataAndRender
    );
  }, function (err) {
    // Something went wrong in initialization.
    console.log('Error while Initializing: ' + err.toString());
  });

  // Uses getVisualSpecificationAsync to build a map of encoding identifiers (specified in the .trex file)
  // to fields that the user has placed on the encoding's shelf.
  // Only encodings that have fields dropped on them will be part of the encodingMap.
  async function getFieldsOnEncoding (worksheet) {
    const visualSpec = await worksheet.getVisualSpecificationAsync();
    const marksCard =
      visualSpec.marksSpecifications[visualSpec.activeMarksSpecificationIndex];
    const encodings = [];
    for (const encoding of marksCard.encodings) {
      if (encoding.id === 'drop') {
        encodings.push(encoding.field.name);
      }
    }

    return encodings;
  }

  async function updateDataAndRender () {
    const fields = await getFieldsOnEncoding(tableau.extensions.worksheetContent.worksheet);

    // Clear the content region before we render
    const content = document.getElementById('content');
    content.innerHTML = '';

    const msg = JSON.stringify(fields);

    // Display the array of fields mapped to the encoding ("drop")
    content.innerHTML = msg;


    // To do: 
    // Add encodings to Marks card (for example, x, y, and r coordinates)
    // Call worksheet.getSummaryDataReaderAsync() to get worksheet data
    // Combine the worksheet data with the encodings (x, y, r)
    // Pass the coordinates and data to code that generates visualization
    // See the connectedScatterplot sample for an example
  }
 })();

```

然后在 Viz 扩展中，选择添加扩展。在出现的添加扩展对话框中，选择访问本地扩展。选择我们刚才写的扩展，将一些字段拖放到标记卡上剪头所指的图标上。您添加到标记卡的字段名称会在工作表中显示。每次添加或删除字段时，名称都会更新。

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NzZiNjFjYWYwNTQzMjEzNzJhY2FlNjVmNWEzNjFjY2NfbzZuNHZaVzZuT0pKM3JGVmJDc1JSSUMwZXFTNllKc1ZfVG9rZW46RW5pSWJLTVRZb296M2J4TFY5YWNxOGZpbm5BXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

### 原理解释

这个示例说明了在您的可视化扩展中需要包含的一些常见组件。详细的过程如下：

1、初始化扩展 API

当 HTML 页面添加到工作表时， `window.onload` 事件调用代码初始化 Extensions API（ `initializeAsync` ）。在视图扩展初始化期间，代码使用 `worksheetContent` 对象访问当前 `worksheet` 。在后续调用中使用 `worksheet` 对象。如果由于某种原因初始化失败，例如，您正在使用不支持视图扩展的较旧版本的库（ `<worksheet-extension>` ），则错误将写入控制台日志。

2、向工作表添加事件监听器以捕捉变化

在初始化代码中，代码为 `tableau.TableauEventType.SummaryDataChanged` 事件添加了一个事件监听器，并指定了一个名为 `updateDataAndRender` 的事件处理器。每当工作表的数据发生变化时，事件处理器被调用以重新绘制可视化。例如，每当在标记卡上的编码瓷砖上拖放字段时，工作表数据就会发生变化。

3、创建事件处理器以获取数据和编码，并渲染视图

这是根据当前编码和数据绘制视图的代码。这个示例非常简化，仅使用了调用的方法来获取编码， `worksheet.getVisualSpecificationAsync()` 。从 `visualSpec` ，收集了从标记卡获取的编码列表。HelloVizExtensions 示例只有一个编码标签，显示文本为"Drop..."。在这个图标上放置的字段列表显示在工作表的内容区域中。在实际的扩展中，你可能会使用多个编码，它们对应于你可视化中的坐标（例如，x、y 和 r）。

这段代码示例中缺失的另一部分是数据。要从工作表中获取数据，您调用 `worksheet.getSummaryDataReaderAsync()` 方法。此方法返回一个 `DataTableReader` ，您使用它来浏览工作表中的汇总数据，从中提取数据行。请参阅 connecteScatterplot 可视化扩展示例，以了解如何实现这一点。

最后，当你有了数据和编码到字段的地图后，你可以将这些信息传递给可以生成散点图、Sankey 图或网络可视化代码。**这个示例只是将字段打印到工作表的内容区域。**但这只是一个开始。

## 创建雷达图扩展

### 所需材料

我这里用到了 ai 来辅助代码的编写，效果还可以，

主要用到的工具有

- cursor https://www.cursor.com/ 编码软件，
    
- deepseek ai https://chat.deepseek.com/sign_in 模型，
    

Cursor 配置 deepseek模型的教程是 https://juejin.cn/post/7400945359194210316

- chrome https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/706915/ 调试代码。
    

用到的数据是

科目 分数

语文 57

数学 66

外语 71

物理 63

化学 63

生物 86

政治 78

历史 70

用到的雷达图参考代码是

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Radar Chart with D3.js</title>
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background-color: #f0f0f0;
    }
    .container svg {
      background-color: #ffffff;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .main .webs polygon {
      fill: none;
      stroke: #ccc;
      stroke-width: 1;
    }
    .main .lines line {
      stroke: #ccc;
      stroke-width: 1;
    }
    .main .areas polygon {
      fill-opacity: 0.5;
      stroke-width: 2;
    }
    .main .texts text {
      font-size: 12px;
      text-anchor: middle;
      dominant-baseline: middle;
    }
  </style>
</head>
<body>
  <div class="container">
    <svg width="600" height="300"></svg>
  </div>
  <script>
    window.onload = function() {
      var width = 600, height = 300;
      // 创建一个分组用来组合要画的图表元素
      var main = d3.select('.container svg').append('g')
              .classed('main', true)
              .attr('transform', "translate(" + width/2 + ',' + height/2 + ')');
      // 模拟数据
      var data = {
          fieldNames: ['语文','数学','外语','物理','化学','生物','政治','历史'],
          values: [
              [10,20,30,40,50,60,70,80]
          ]
      };
      // 设定一些方便计算的常量
      var radius = 100,
      // 指标的个数，即fieldNames的长度
              total = 8,
      // 需要将网轴分成几级，即网轴上从小到大有多少个正多边形
              level = 4,
      // 网轴的范围，类似坐标轴
              rangeMin = 0,
              rangeMax = 100,
              arc = 2 * Math.PI;
      // 每项指标所在的角度
      var onePiece = arc/total;
      // 计算网轴的正多边形的坐标
      var polygons = {
          webs: [],
          webPoints: []
      };
      for(var k=level;k>0;k--) {
          var webs = '',
                  webPoints = [];
          var r = radius/level * k;
          for(var i=0;i<total;i++) {
              var x = r * Math.sin(i * onePiece),
                      y = r * Math.cos(i * onePiece);
              webs += x + ',' + y + ' ';
              webPoints.push({
                  x: x,
                  y: y
              });
          }
          polygons.webs.push(webs);
          polygons.webPoints.push(webPoints);
      }

      console.log(data)

      // 绘制网轴
      var webs = main.append('g')
              .classed('webs', true);
      webs.selectAll('polygon')
              .data(polygons.webs)
              .enter()
              .append('polygon')
              .attr('points', function(d) {
                  return d;
              });
      // 添加纵轴
      var lines = main.append('g')
              .classed('lines', true);
      lines.selectAll('line')
              .data(polygons.webPoints[0])
              .enter()
              .append('line')
              .attr('x1', 0)
              .attr('y1', 0)
              .attr('x2', function(d) {
                  return d.x;
              })
              .attr('y2', function(d) {
                  return d.y;
              });
      // 计算雷达图表的坐标
      var areasData = [];
      var values = data.values;
      for(var i=0;i<values.length;i++) {
          var value = values[i],
                  area = '',
                  points = [];
          for(var k=0;k<total;k++) {
              var r = radius * (value[k] - rangeMin)/(rangeMax - rangeMin);
              var x = r * Math.sin(k * onePiece),
                      y = r * Math.cos(k * onePiece);
              area += x + ',' + y + ' ';
              points.push({
                  x: x,
                  y: y
              })
          }
          areasData.push({
              polygon: area,
              points: points
          });
      }
      // 添加g分组包含所有雷达图区域
      var areas = main.append('g')
              .classed('areas', true);
      // 添加g分组用来包含一个雷达图区域下的多边形以及圆点
      areas.selectAll('g')
              .data(areasData)
              .enter()
              .append('g')
              .attr('class',function(d, i) {
                  return 'area' + (i+1);
              });
      for(var i=0;i<areasData.length;i++) {
          // 依次循环每个雷达图区域
          var area = areas.select('.area' + (i+1)),
                  areaData = areasData[i];
          // 绘制雷达图区域下的多边形
          area.append('polygon')
                  .attr('points', areaData.polygon)
                  .attr('stroke', function(d, index) {
                      return getColor(i);
                  })
                  .attr('fill', function(d, index) {
                      return getColor(i);
                  });
          // 绘制雷达图区域下的点
          var circles = area.append('g')
                  .classed('circles', true);
          circles.selectAll('circle')
                  .data(areaData.points)
                  .enter()
                  .append('circle')
                  .attr('cx', function(d) {
                      return d.x;
                  })
                  .attr('cy', function(d) {
                      return d.y;
                  })
                  .attr('r', 3)
                  .attr('stroke', function(d, index) {
                      return getColor(i);
                  });
      }
      // 计算文字标签坐标
      var textPoints = [];
      var textRadius = radius + 20;
      for(var i=0;i<total;i++) {
          var x = textRadius * Math.sin(i * onePiece),
                  y = textRadius * Math.cos(i * onePiece);
          textPoints.push({
              x: x,
              y: y
          });
      }
      // 绘制文字标签
      var texts = main.append('g')
              .classed('texts', true);
      texts.selectAll('text')
              .data(textPoints)
              .enter()
              .append('text')
              .attr('x', function(d) {
                  return d.x;
              })
              .attr('y', function(d) {
                  return d.y;
              })
              .text(function(d,i) {
                  return data.fieldNames[i];
              });
    };
    function getColor(idx) {
      var palette = [
          '#2ec7c9', '#b6a2de', '#5ab1ef', '#ffb980', '#d87a80',
          '#8d98b3', '#e5cf0d', '#97b552', '#95706d', '#dc69aa',
          '#07a2a4', '#9a7fd1', '#588dd5', '#f5994e', '#c05050',
          '#59678c', '#c9ab00', '#7eb00a', '#6f5553', '#c14089'
      ]
      return palette[idx % palette.length];
    }
  </script>
</body>
</html>
```

### Ai 辅助编程

举例来讲，首先我告诉 ai ，什么是可视化扩展，然后告诉它我想实现一个 tableau 的雷达图可视化扩展

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ODQ0OGM1ZjFjNjk1NDRmZWQyMmMxM2E0NzY0MWQ1OGRfd0RIOWhZb21DcWl4QzRIbTZhdmtEeXJkaVBSeERCaTJfVG9rZW46TUM4TmI4bmtJb2VwTlN4MzFwQ2M1NERtbmZoXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

这是我跟它的第一轮对话，ai 会给你一些代码参考，我一般直接复制拿来用：

connectedScatterplot.trex, connectedScatterplot.html, connectedScatterplot.js 3个文件共同构建了 tableau viz extension程序，它调用d3库和tableau.extensions的api，在tableau实现在tableau展现新的可视化，radar.html是调用d3实现的雷达图可视化， 现在请参考我给你的这几个文件，帮我创建一个tableau viz extension程序 1、程序的结构是类似于connectedScatterplot.trex, connectedScatterplot.html, connectedScatterplot.js 2、程序的样式类似于radar.html 3、这个扩展程序叫radar，它由radar 文件夹下面的radar.trex, radar.html, radar.js 三个文件共同构成，实现的效果是在tableau中展现雷达图，

### 调试方法

有了代码之后，现在可以运行它，然后需要调试，看代码中的报错

在 tableau 的快捷方式中，右键，属性，添加以下代码

-remote-debugging-port=8696

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=MzQ2ZTVlMGFkZDkxNDNiZWZhMjA2YjQ1OTFkZTgxMGZfalRvY1hkODJmU1lSUDQ2UVNLbW1SZVRiQVVnM2JuT1FfVG9rZW46WXptUGI3VUhkb1d4Vkh4bFVnNGNyNTNNbmg3XzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

在标记工具栏，添加扩展程序，然后选择本地扩展，打开我们刚才编写的扩展

现在再打开上面下载的 chrome，访问 http://localhost:8696/

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDkxNTA5ZjY3NjcxM2ZlZWI3YzNlM2JmNThhOWVlOWFfYkd3ekJzdlg4TzJXdEJVUUgxZjFMOWhwZFkzRXI5dktfVG9rZW46TW5CaWJSeDFQbzBpTkJ4OXhFWGNzbktMblplXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

### 调试过程

多刷新几次，选择第一个雷达图，然后就可以排查错误

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=YzA5YjlkNDJmZjFkOTAxYTA5NjI0ZTcyMzFmMTBlOWNfWUU4RDYzT1FLZlBIVUpKYVFDSlkwNThxeHZzVUEydGlfVG9rZW46S0JEYWJ0ZjJLbzdhWEx4RG4zOGNVSzg4bmFiXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

比如这里出现的错误是说dashboard没有定义

对应第一行代码

```JavaScript
const worksheet = 
tableau.extensions.dashboardContent.dashboard.worksheets.find(w => w.name === 'RadarData');
```

遇到的错误继续问 ai 它会告诉你怎么改

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=M2Y0OTJjODc4Mjk0ZjllMzAyM2Y3YWE5YzE3YjIwMTBfUDhsVmo0WDlia0VuNW5xZ2ZVUHlPWWhmV2JCQnpjREJfVG9rZW46Um1XQWJUMUVlbzRmTmJ4dXZoRmNLcmE4bmJIXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

修改完代码之后，重新添加一次扩展程序才能继续查看，每修改一次都需要进行这个操作

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDdhOWJiMzViZjI5OThjNmQ3MTc0OTU1NTQ3NGIzMmFfb2g2SGFQbGp5cnJQMzkxTlgwUDZ0bk9RRVhSVDhOak9fVG9rZW46Tk5PNmJtMkFYb0RXY1h4TzdxUWNDelZobmhjXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

我们注意到现在的报错跟之前不一样，并且右边的 chrome 显示的就是 tableau 工作表的实时画面

并且通过点击查看数据还发现实际上 tableau 已经拿到了数据，那就说明是渲染有问题

我们继续根据这些报错信息询问 ai ，它建议将拿到的数据打印出来

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NWZhNDdjOGZlMDVlYmU5NmFmNGNiOTJjYWE2YzdlYzBfclIxWVZBZGRWdEJwcExWOHFCRGdvSGU0U1U1clQ4UHNfVG9rZW46V0pKZ2IxWHJKb2ZiN1N4cUN3SWNmcTBSbkpjXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=OTljMjdmODY3NWYzNDI0ZmUwNmQxMmQ4ZjVjNzg4OGZfM21SaEJ4ZVJqOFJ6YXhOQ1haRGdBVG1lTHh5R21ZbFNfVG9rZW46U3NYeWJkbm93b0lvN1Z4N1RCZGNmbjFkbnlkXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

将数据打印出来之后发现数据能够顺利拿到，值是正确的

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=M2UwMThmZjk0OGY2YTU0NTIzMzdiYjVhMmMyMjA4ZWVfRlJmUkxubGFJZGpKb0cxZGRUcE1TZldXM0Y3aURuNm9fVG9rZW46TU56cGJrZVBFb3QyeHp4QUYzY2N4NXZFbmxmXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

现在应该就是渲染的问题，又去查看了参考的雷达图的源代码发现那里的数据结构不太一样，再经过几轮询问 ai，最终将拿到的数据转换一下结构，成功得到了雷达图。

注意：如果反复询问 ai ，都不能排错，这时建议换个方向问会有奇效，有时候甚至需要完全推翻重来。

![](https://danyueqingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NmVkNmQxOTg0NmMwMjUyNjI2ZjkzOWU3NWEzYzU5MDVfZXVrWUZoa0dPUklGVk8xWnFlc1dLTzhTbGlmb1Jyc0pfVG9rZW46VWhJemJFMTBJb0lvaEx4S2I0Y2NtamxmbkZlXzE3MjQ5MDc5NTI6MTcyNDkxMTU1Ml9WNA)

### 源码

Trex

```XML
<?xml version="1.0" encoding="utf-8"?>
<manifest manifest-version="0.1" xmlns="http://www.tableau.com/xml/extension_manifest">
  <worksheet-extension id="com.tableau.extension.demo.radar" extension-version="1.0.0">
    <default-locale>en_US</default-locale>
    <name resource-id="name"/>
    <description>雷达图</description>
    <author name="tableau" email="github@tableau.com" organization="tableau" website="https://www.tableau.com"/>
    <min-api-version>1.11</min-api-version>
    <source-location>
      <url>http://localhost:8765/Samples/Viz/Radar/radar.html</url>
    </source-location>
    <icon/>
    <encoding id="x">
      <display-name>X</display-name>
      <data-spec>
        <data-type>numeric</data-type>
      </data-spec>
      <role-spec>
        <role-type>continuous-measure</role-type>
      </role-spec>
      <fields max-count="1"/>
      <encoding-icon token="letter-x"/>
      <tooltip resource-id="tooltip-x-id">Default tooltip text for X encoding</tooltip>
    </encoding>
    <!-- <encoding id="y">
      <display-name>Y</display-name>
      <data-spec>
        <data-type>numeric</data-type>
      </data-spec>
      <role-spec>
        <role-type>continuous-measure</role-type>
      </role-spec>
      <fields max-count="1"/>
      <encoding-icon token="letter-y"/>
      <tooltip resource-id="tooltip-y-id">Default tooltip text for Y encoding</tooltip>
    </encoding> -->
    <encoding id="text">
      <display-name>Text</display-name>
      <fields max-count="1"/>
      <encoding-icon token="text"/>
      <tooltip resource-id="tooltip-text-id">Default tooltip text for Text encoding</tooltip>
    </encoding>
  </worksheet-extension>
  <resources>
    <resource id="name">
      <text locale="en_US">雷达图</text>
    </resource>
    <resource id="tooltip-x-id">
      <text locale="en_US">X轴的默认提示文本</text>
    </resource>
    <!-- <resource id="tooltip-y-id">
      <text locale="en_US">Y轴的默认提示文本</text>
    </resource> -->
    <resource id="tooltip-text-id">
      <text locale="en_US">文本的默认提示文本</text>
    </resource>
  </resources>
</manifest>
```

Html

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Radar Chart with D3.js</title>
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <script src="../../../lib/tableau.extensions.1.latest.js"></script>
  <script src="./radar.js"></script>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background-color: #f0f0f0;
    }
    .container svg {
      background-color: #ffffff;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .main .webs polygon {
      fill: none;
      stroke: #ccc;
      stroke-width: 1;
    }
    .main .lines line {
      stroke: #ccc;
      stroke-width: 1;
    }
    .main .areas polygon {
      fill-opacity: 0.5;
      stroke-width: 2;
    }
    .main .texts text {
      font-size: 12px;
      text-anchor: middle;
      dominant-baseline: middle;
    }
  </style>
</head>
<body>
  <div class="container" id="content">
    <svg width="600" height="300"></svg>
  </div>
</body>
</html>
```

Js

```JavaScript
'use strict';
/* global d3 */

(function () {
  window.onload = tableau.extensions.initializeAsync().then(() => {
    const worksheet = tableau.extensions.worksheetContent.worksheet;
    let summaryData = {};
    let encodingMap = {};

    const updateDataAndRender = async () => {
      [summaryData, encodingMap] = await Promise.all([
        getSummaryDataTable(worksheet),
        getEncodingMap(worksheet)
      ]);

      renderRadarChart(summaryData, encodingMap);
    };

    onresize = () => renderRadarChart(summaryData, encodingMap);

    worksheet.addEventListener(
      tableau.TableauEventType.SummaryDataChanged,
      updateDataAndRender
    );

    updateDataAndRender();
  });

  async function getSummaryDataTable(worksheet) {
    const summaryData = await worksheet.getSummaryDataAsync();
    const rows = convertToListOfNamedRows(summaryData);
    console.log('Summary Data:', rows); // 打印获取的数据

    // 转换为 ra.html 中的数据结构
    const keys = Object.keys(rows[0]);
    const fieldNames = rows.map(row => row[keys[0]]._value);
    const values = keys.slice(1).map(key => rows.map(row => row[key]._value));
    return { fieldNames, values };
  }

  function convertToListOfNamedRows(dataTablePage) {
    const rows = [];
    const fieldNames = dataTablePage.columns.map(column => column.fieldName);
    dataTablePage.data.forEach(row => {
      const namedRow = {};
      row.forEach((value, index) => {
        namedRow[fieldNames[index]] = value;
      });
      rows.push(namedRow);
    });
    console.log('Converted Data:', rows); // 打印转换后的数据
    return rows;
  }

  async function getEncodingMap(worksheet) {
    const visualSpec = await worksheet.getVisualSpecificationAsync();

    const encodingMap = {};

    if (visualSpec.activeMarksSpecificationIndex < 0) return encodingMap;

    const marksCard =
      visualSpec.marksSpecifications[visualSpec.activeMarksSpecificationIndex];
    for (const encoding of marksCard.encodings) {
      if (encoding.id === 'x' || encoding.id === 'text') {
        encodingMap[encoding.id] = encoding.field;
      }
    }

    return encodingMap;
  }

  function renderRadarChart(data) {
    // 确保 data 是一个对象并且包含 fieldNames 和 values 字段
    if (!data || !data.fieldNames || !data.values) {
      console.error('数据格式不正确');
      return;
    }

    const { fieldNames, values } = data;
    const width = 600, height = 300;

    d3.select('.container svg').selectAll('*').remove();

    const main = d3.select('.container svg').append('g')
      .classed('main', true)
      .attr('transform', `translate(${width / 2}, ${height / 2})`);

    const radius = 100;
    const total = fieldNames.length;
    const rangeMin = 0;
    const rangeMax = 100;
    const arc = 2 * Math.PI;
    const onePiece = arc / total;

    // 计算网轴的正多边形的坐标
    const polygons = {
      webs: [],
      webPoints: []
    };

    const level = 4;
    for (let k = level; k > 0; k--) {
      let webs = '', webPoints = [];
      const r = radius / level * k;
      for (let i = 0; i < total; i++) {
        const x = r * Math.sin(i * onePiece);
        const y = r * Math.cos(i * onePiece);
        webs += `${x},${y} `;
        webPoints.push({ x, y });
      }
      polygons.webs.push(webs);
      polygons.webPoints.push(webPoints);
    }

    // 绘制网轴
    const webs = main.append('g')
      .classed('webs', true);
    webs.selectAll('polygon')
      .data(polygons.webs)
      .enter()
      .append('polygon')
      .attr('points', d => d);

    // 添加纵轴
    const lines = main.append('g')
      .classed('lines', true);
    lines.selectAll('line')
      .data(polygons.webPoints[0])
      .enter()
      .append('line')
      .attr('x1', 0)
      .attr('y1', 0)
      .attr('x2', d => d.x)
      .attr('y2', d => d.y);

    // 计算雷达图表的坐标
    const areasData = [];
    const value = values[0]; // 这里假设只有一个值数组
    let area = '', points = [];
    for (let k = 0; k < total; k++) {
      const r = radius * (value[k] - rangeMin) / (rangeMax - rangeMin);
      const x = r * Math.sin(k * onePiece);
      const y = r * Math.cos(k * onePiece);
      area += `${x},${y} `;
      points.push({ x, y });
    }
    areasData.push({ polygon: area, points });

    // 添加g分组包含所有雷达图区域
    const areas = main.append('g')
      .classed('areas', true);
    areas.selectAll('g')
      .data(areasData)
      .enter()
      .append('g')
      .attr('class', (d, i) => `area${i + 1}`);

    areasData.forEach((areaData, i) => {
      const area = areas.select(`.area${i + 1}`);
      area.append('polygon')
        .attr('points', areaData.polygon)
        .attr('stroke', getColor(i))
        .attr('fill', getColor(i));

      const circles = area.append('g')
        .classed('circles', true);
      circles.selectAll('circle')
        .data(areaData.points)
        .enter()
        .append('circle')
        .attr('cx', d => d.x)
        .attr('cy', d => d.y)
        .attr('r', 3)
        .attr('stroke', getColor(i));
    });

    // 计算文字标签坐标
    const textPoints = [];
    const textRadius = radius + 20;
    for (let i = 0; i < total; i++) {
      const x = textRadius * Math.sin(i * onePiece);
      const y = textRadius * Math.cos(i * onePiece);
      textPoints.push({ x, y });
    }

    // 绘制文字标签
    const texts = main.append('g')
      .classed('texts', true);
    texts.selectAll('text')
      .data(textPoints)
      .enter()
      .append('text')
      .attr('x', d => d.x)
      .attr('y', d => d.y)
      .text((d, i) => fieldNames[i]);
  }

  function getColor(idx) {
    const palette = [
      '#2ec7c9', '#b6a2de', '#5ab1ef', '#ffb980', '#d87a80',
      '#8d98b3', '#e5cf0d', '#97b552', '#95706d', '#dc69aa',
      '#07a2a4', '#9a7fd1', '#588dd5', '#f5994e', '#c05050',
      '#59678c', '#c9ab00', '#7eb00a', '#6f5553', '#c14089'
    ];
    return palette[idx % palette.length];
  }
})();
```



<Comment />