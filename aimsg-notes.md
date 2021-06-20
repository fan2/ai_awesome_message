
## 代码结构

向外导出：awesome_message.dart

主要实现：

- awesome_helper.dart：定义了 class `AwesomeHelper` 工具类；  
- awesome_message.dart：定义了 class `AwesomeMessage` extends StatefulWidget；  
- awesome_message_route.dart：定义了 class `AwesomeMessageRoute` extends OverlayRoute；  

示例依赖 [airoute](https://github.com/pdliuw/airoute)。

## 核心接口

AwesomeHelper.createAwesome 返回 AwesomeMessage。

_AwesomeMessageState.build 返回 Align(child: _getAwesomeMessage(),)。  
具体参考 _generateAwesomeMessage->_getAppropriateRowLayout 中 widget.icon 非空的布局。  

```Dart
// awesome_message.dart
  Widget _getAwesomeMessage() {
    Widget awesomeMessage;

    if (widget.userInputForm != null) {
      awesomeMessage = _generateInputAwesomeMessage();
    } else {
      awesomeMessage = _generateAwesomeMessage();
    }

    return Stack(
      children: [
        FutureBuilder(
          future: _boxHeightCompleter.future,
          builder: (context, AsyncSnapshot<Size> snapshot) {

          }
        ),
        awesomeMessage,
      ]
    )
```

## 使用方式

### 作为Widget使用

```Dart
          Container(
            child:
                AwesomeHelper.createAwesome(title: "title", message: "message"),
          ),
```

### 作为Route使用

```Dart
          Navigator.push(
            context,
            AwesomeMessageRoute(
              awesomeMessage: AwesomeHelper.createAwesome(
                  title: "title", message: "message"),
              theme: null,
              settings: RouteSettings(name: "/messageRouteName"),
            ),
          );
```

## 示例效果

首页（main.dart）点击 next 按钮，打开 PopupPage（popup_page.dart）。

### PopupPage

Tools 是一个下拉 ExpansionTile，其中 Switch 控制变量 _showMessageWidget 决定是否在页面中间展示作为 Widget 使用的效果。

```Dart
          Container(
            child: _showMessageWidget
                ? AwesomeHelper.createAwesome(
                    title: "title", message: "message")
                : Text(""),
          ),
```

![aimsg-example-tool](images/aimsg-example-tool.jpeg)

### ToolPage

在 PopupPage 页面点击 online config tools 按钮，打开 AiAwesomeMessageToolPage 页面（tool_page.dart）。

- ToggleButtons-GROUNDED/FLOATING：控制是否全屏覆盖顶部状态栏；  
- Margin：控制 message toast 两侧边距；  
- Indicator show：控制是否显示左侧显示条；  
- ToggleButtons-info/warn/error/complete：控制显示消息的类型；  

![aimsg-example-popup](images/aimsg-example-popup.jpeg)
