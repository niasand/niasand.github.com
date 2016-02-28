---
title: Appium API Note
date: 2016-02-28 17:24:15
tags: appium
---

### Appium API(Python版)笔记

锁定屏幕

``` python
driver.lock(5)
```
把当前应用放到后台去

```python
driver.background_app(5)
```
在 iOS 上收起键盘

```python
driver.hide_keyboard()
```

检查应用是否已经安装

```python
driver.is_app_installed('com.example.android.apis')
```
安装应用到设备中去

```python
driver.install_app('path/to/my.apk')
```
从设备中删除一个应用

```python
driver.remove_app('com.example.android.apis')
```
模拟设备摇晃

```python
driver.shake()
```
关闭应用

```python
driver.close_app();
```
启动应用

```python
driver.launch_app()
```

应用重置

```python
driver.reset()
```

列出所有的可用上下文

```python
driver.contexts
```

列出当前上下文

```python
driver.current_context
```

将上下文切换到默认上下文

```python
driver.switch_to.context(None)
```

iOS 里是 Localizable.strings Android 里是 strings.xml

```python
driver.app_strings
```
发送一个按键事件给设备

```python
driver.keyevent(176)
```
Android only 得到当前 activity。

```python
driver.current_activity
```
触摸动作 / 多点触摸动作

```python
action = TouchAction(driver)
action.press(element=el, x=10, y=10).release().perform()
```

模拟用户滑动
```python
driver.swipe(75, 500, 75, 0, 0.8)
```
Places two fingers at the edges of the screen and brings them together. 在 0% 到 100% 内双指缩放屏幕

```python
driver.pinch(element=el)
```

放大屏幕 在 100% 以上放大屏幕

```python
driver.zoom(element=el)
```

从设备中拉出文件
```python
driver.pull_file('Library/AddressBook/AddressBook.sqlitedb')
```
推送文件到设备中

```python
data = "some data for the file"
path = "/data/local/tmp/file.txt"
driver.push_file(path, data.encode('base64'))
```

<!--more-->

打印页面元素

```python
driver.page_source
```
获取元素text属性

```python
element.get_attribute('text')     android使用(获取元素的text值)
element.text     ios使用(获取元素的value值)
```

定位

```python
driver.find_element_by_ios_uiautomation(uia_string)
driver.find_elements_by_ios_uiautomation(uia_string)
driver.find_element_by_android_uiautomator(uia_string)
driver.find_elements_by_android_uiautomator(uia_string)
driver.find_element_by_android_uiautomator('new UiSelector().text("竞彩篮球")')
driver.find_element_by_accessibility_id(id)
driver.find_elements_by_accessibility_id(id)
driver.find_element_by_id(id)
driver.find_elements_by_id(id)
driver.find_element_by_xpath(xpath)
driver.find_elements_by_xpath(xpath)
driver.find_element_by_name(name)
driver.find_elements_by_name(name)
driver.find_element_by_class_name(class)
driver.find_elements_by_class_name(class)
driver.find_element(by,value)
by:"id"、"xpath"、"class name"、 "name"、'-ios uiautomation'、'-android uiautomator'、'accessibility id'
```

坐标点击

```python
driver.tap([(x,y)],time)
```

输入

```python
driver.send_keys()
driver.set_text() Android可用
driver.set_value() ios可用
```


截图

```python
driver.get_screenshot_as_file(filename)
```
获取手机屏幕分辨率

```python
driver.get_window_size()
x = pythondriver.get_window_size()['width']
y = driver.get_window_size()['height']
```

设置屏幕分辨率

```python
driver.set_window_size(width,height)
```

获取当前坐标位置

```python
driver.get_window_position()
```
滚动

```python
driver.scroll(ele1,ele2)
```
按住element并拖动到另外一个element上

```python
driver.drag_and_drop(ele1,ele2)
```
缩小

```python
driver.pinch(ele)
driver.zoom(ele)
```
重启app

```python
driver.reset()
```
隐藏键盘

```python
driver.hide_keyboard()
```
发送键盘事件
```python
driver.keyevent(keycode)
```
按住键盘

```python
driver.press_keycode(keycode)
```
长按住键盘

```python
driver.long_press_keycode(keycode)
```
上传文件

```python
driver.push_file(path)
```
下载文件

```python
driver.pull_file(path)
```
下载文件夹

```python
driver.pull_folder(path)
```
app隐藏后台

```python
driver.background_app(time)
```
安装app

```python
driver.install_app(path)
```
卸载app

```python
driver.remove_app(app_id)
```
启动app

```python
driver.launch_app()
```
关闭app

```python
driver.close_app()
```
启动activity

```python
driver.start_activity(app_package, app_activity)
```
打印当前activity

```python
driver.current_activity
```
锁屏

```python
driver.lock(time)
```

振动
```python
driver.shake()
```
打开通知栏(api 18以上)

```python
driver.open_notifications()
```
获取网络

```python
driver.network_connection
```
设置网络连接( Android only.)

```python
driver.set_network_connection(type)
```
|Value (Alias) | Data | Wifi | Airplane Mode|
----|------|----
|0(None) | 0 | 0 | 0|
|1(Airplane Mode) | 0 | 0 | 1|
|2(Wifi only) | 0 | 1 | 0|
|4(Data only) | 1 | 0 | 0|
|6(All network on) | 1 | 1 | 0|

type参数：

NO_CONNECTION = 0

AIRPLANE_MODE = 1

WIFI_ONLY = 2

DATA_ONLY = 4

ALL_NETWORK_ON = 6

```python
from appium.webdriver.connectiontype import ConnectionType
driver.set_network_connection(ConnectionType.AIRPLANE_MODE)
```

获取手机输入法（返回list）

```python
driver.available_ime_engines
```
激活某种输入法

```python
driver.activate_ime_engine(engine)
```
判断输入法是否激活(返回bool)

```python
driver.is_ime_active()
```
撤销当前输入法(Android only)

```python
driver.deactivate_ime_engine()
```
得到当前设置

```python
driver.get_settings()
```
返回{u'ignoreUnimportantViews': False}

更新当前设置

```python
driver.update_settings(settings)
```
settings参数为dict，如{ignoreUnimportantViews : True}

ignoreUnimportantViews
 参数：调用 uiautomator 的函数setCompressedLayoutHierarchy()。由于
 Accessibility 命令在忽略部分元素的情况下执行速度会加快，这个关键字能加快测试执行的速度。被忽略的元素将不能够被找到，因此这个关键字同时也被实现成可以随时改变的 *设置 ( settings ) * 。默认值 false


开关定位服务

```python
driver.toggle_location_services()
```
