.. _locating-elements:

查找元素
========

在一个页面中有很多不同的策略可以定位一个元素。在你的项目中，
你可以选择最合适的方法去查找元素。Selenium提供了下列的方法给你:

- `find_element_by_id`
- `find_element_by_name`
- `find_element_by_xpath`
- `find_element_by_link_text`
- `find_element_by_partial_link_text`
- `find_element_by_tag_name`
- `find_element_by_class_name`
- `find_element_by_css_selector`


**一次查找多个元素 (这些方法会返回一个list列表):**

- `find_elements_by_name`
- `find_elements_by_xpath`
- `find_elements_by_link_text`
- `find_elements_by_partial_link_text`
- `find_elements_by_tag_name`
- `find_elements_by_class_name`
- `find_elements_by_css_selector`


除了上述的公共方法，下面还有两个私有方法，在你查找也页面元素的时候也许有用。
他们是 `find_element` 和 `find_elements` 。

用法示例::

  from selenium.webdriver.common.by import By

  driver.find_element(By.XPATH, '//button[text()="Some text"]')
  driver.find_elements(By.XPATH, '//button')


下面是 `By` 类的一些可用属性::

    ID = "id"
    XPATH = "xpath"
    LINK_TEXT = "link text"
    PARTIAL_LINK_TEXT = "partial link text"
    NAME = "name"
    TAG_NAME = "tag name"
    CLASS_NAME = "class name"
    CSS_SELECTOR = "css selector"


通过ID查找元素
~~~~~~~~~~~~~~

当你知道一个元素的 `id` 时，你可以使用本方法。在该策略下，页面中第一个该 `id` 元素
会被匹配并返回。如果找不到任何元素，会抛出 ``NoSuchElementException`` 异常。

作为示例，页面元素如下所示::

  <html>
   <body>
    <form id="loginForm">
     <input name="username" type="text" />
     <input name="password" type="password" />
     <input name="continue" type="submit" value="Login" />
    </form>
   </body>
  <html>

可以这样查找表单(form)元素::

  login_form = driver.find_element_by_id('loginForm')


通过Name查找元素
~~~~~~~~~~~~~~~~

当你知道一个元素的 `name` 时，你可以使用本方法。在该策略下，页面中第一个该 `name` 元素
会被匹配并返回。如果找不到任何元素，会抛出 ``NoSuchElementException`` 异常。


作为示例，页面元素如下所示::

   <html>
    <body>
     <form id="loginForm">
      <input name="username" type="text" />
      <input name="password" type="password" />
      <input name="continue" type="submit" value="Login" />
      <input name="continue" type="button" value="Clear" />
     </form>
   </body>
   <html>

name属性为 username & password 的元素可以像下面这样查找::

  username = driver.find_element_by_name('username')
  password = driver.find_element_by_name('password')

这会得到 "Login" 按钮，因为他在 "Clear" 按钮之前::

  continue = driver.find_element_by_name('continue')


通过XPath查找元素
~~~~~~~~~~~~~~~~~

XPath是XML文档中查找结点的语法。因为HTML文档也可以被转换成XML(XHTML)文档，
Selenium的用户可以利用这种强大的语言在web应用中查找元素。
XPath扩展了（当然也支持）这种通过id或name属性获取元素的简单方式，同时也开辟了各种新的可能性，
例如获取页面上的第三个复选框。

使用XPath的主要原因之一就是当你想获取一个既没有身份证明（identification）属性也没有名字属性的元素时,
你可以通过XPath使用元素的绝对位置来获取他（这是不推荐的),或相对于有一个身份证明（identification）或名字属性的元素
（理论上的父元素)的来获取你想要的元素XPath定位器也可以通过非身份证明（identification）和名字属性查找元素。





绝对的XPath是所有元素都从根元素的位置(HTML)开始定位,只要应用中有轻微的调整,会就导致你的定位失败。
但是通过就近的包含身份证明（identification）或者名字属性的元素出发定位你的元素,这样相对关系就很靠谱,
因为这种位置关系很少改变,所以可以使你的测试更加强大。

作为示例,页面元素如下所示::

   <html>
    <body>
     <form id="loginForm">
      <input name="username" type="text" />
      <input name="password" type="password" />
      <input name="continue" type="submit" value="Login" />
      <input name="continue" type="button" value="Clear" />
     </form>
   </body>
   <html>

可以这样查找表单(表格)元素::

log in _ form = driver . find _ element _ by _ XPath("/html/body/form[1]")
log in _ form = driver . find _ element _ by _ XPath("//form[1]")
log in _ form = driver . find _ element _ by _ XPath("//form[@ id = ' loginForm ']")


1.绝对定位（页面结构轻微调整就会被破坏)

2.超文本标记语言页面中的第一个形式元素

3.包含` id '属性并且其值为` loginForm '的形式元素

用户名元素可以如下获取::

username = driver . find _ element _ by _ XPath("//form/input[@ name = ' username ']")
username = driver . find _ element _ by _ XPath("//form[@ id = ' loginForm ']/input[1]")
username = driver . find _ element _ by _ XPath("//input[@name='username']")

1.第一个形式元素中包含名字属性并且其值为`用户名'的投入元素

2.身份证明（identification）为` loginForm '的形式元素的第一个投入子元素

3.第一个名字属性为`用户名'的投入元素

"清除"按钮可以如下获取::

clear _ button = driver . find _ element _ by _ XPath("//input[@ name = ' continue '][@ type = ' button ']")
clear _ button = driver . find _ element _ by _ XPath("//form[@ id = ' loginForm ']/input[4]")


1.具有名为“名称”的属性和值“继续”的输入，以及
名为“类型”的属性和值“按钮”

2.属性名为的表单元素的第四个输入子元素
` id '和值` loginForm '

这些实例都是一些举出用法,为了学习更多有用的东西,下面这些参考资料推荐给你：

* `W3Schools XPath教程< http://www.w3schools.com/xsl/xpath_intro.asp>`_
* `W3C XPath建议< http://www.w3.org/TR/xpath>`_
* `XPath教程
  <http://www.zvon.org/comp/r/tut-XPath_1.html>`_
-有互动的例子。

还有一些非常有用的插件,可以协助发现元素的XPath:

* `XPath检查器
  <https://addons.mozilla.org/en-US/firefox/addon/1095?id=1095>`_ -
  suggests XPath and can be used to test XPath results.
* `Firebug <https://addons.mozilla.org/en-US/firefox/addon/1843>`_ -
  XPath suggestions are just one of the many powerful features of this
  very useful add-on.
* `XPath Helper
  <https://chrome.google.com/webstore/detail/hgimnogjllphhhkhlmebbmlgjoejdpjl>`_ -
  for Google Chrome


通过链接文本获取超链接
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

当你知道在一个锚标签中使用的链接文本时使用这个。
在该策略下，页面中第一个匹配链接内容锚标签
会被匹配并返回。如果找不到任何元素，会抛出 ``NoSuchElementException`` 异常。

作为示例，页面元素如下所示::

  <html>
   <body>
    <p>Are you sure you want to do this?</p>
    <a href="continue.html">Continue</a>
    <a href="cancel.html">Cancel</a>
  </body>
  <html>

continue.html 超链接可以被这样查找到::

  continue_link = driver.find_element_by_link_text('Continue')
  continue_link = driver.find_element_by_partial_link_text('Conti')


通过标签名查找元素
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

当你向通过标签名查找元素时使用这个。
在该策略下，页面中第一个匹配该标签名的元素
会被匹配并返回。如果找不到任何元素，会抛出 ``NoSuchElementException`` 异常。

作为示例，页面元素如下所示::

  <html>
   <body>
    <h1>Welcome</h1>
    <p>Site content goes here.</p>
  </body>
  <html>

h1 元素可以如下查找::

  heading1 = driver.find_element_by_tag_name('h1')


通过Class name 定位元素
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

当你向通过class name查找元素时使用这个。
在该策略下，页面中第一个匹配该class属性的元素
会被匹配并返回。如果找不到任何元素，会抛出 ``NoSuchElementException`` 异常。

作为示例，页面元素如下所示::

  <html>
   <body>
    <p class="content">Site content goes here.</p>
  </body>
  <html>

p 元素可以如下查找::

  content = driver.find_element_by_class_name('content')

通过CSS选择器查找元素
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

当你向通过CSS选择器查找元素时使用这个。
在该策略下，页面中第一个匹配该CSS 选择器的元素
会被匹配并返回。如果找不到任何元素，会抛出 ``NoSuchElementException`` 异常。

作为示例，页面元素如下所示::

  <html>
   <body>
    <p class="content">Site content goes here.</p>
  </body>
  <html>

p 元素可以如下查找::

  content = driver.find_element_by_css_selector('p.content')

`Sauce 实验室有一篇很好的文档来介绍CSS选择器 <http://saucelabs.com/resources/selenium/css-selectors>`_
