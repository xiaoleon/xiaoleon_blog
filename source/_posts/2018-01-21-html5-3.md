---
title: H5(3) 表单
date: 2018-01-21 21:12:39
categories:
- HTML5抄书笔记
tags:
- 表单属性
- input类型
- 表单验证
---

在创建Web应用程序的时候，免不了会用到大量的表单元素。在HTML5标准中，吸纳了Web Forms 2.0的标准，大幅度强化了针对表单元素的功能，使得关于表单的开发更快、更方便。

<!-- More -->

### 一、新增元素与属性

#### 1. form属性

在HTML4中，表单内的从属元素必须书写在表单内部，但是在HTML5中，可以把它们书写在页面上任何地方，然后给该元素指定一个form属性，属性值为该表单的id，这样就可以声明该元素从属于指定表单了。

```html
<form id="testform">
    <input type="text">
</form>
<textarea form="testform"></textarea>
```

#### 2. formaction属性

在HTML4中，一个表单内的所有元素都只能通过表单的`action`属性统一提交到另一个页面，而在HTML5中可以给所有的提交按钮，诸如`<input type="submit">`、`<input type="image">`、`<button type="submit">`都增加不同的`formaction`属性，可以将表单提交到不同的页面。

```html
<form id="testform" action="serve.php">
    <input type="submit" name="s1" value="v1" formaction="s1.php">
    <input type="submit" name="s2" value="v2" formaction="s2.php">
    <input type="submit" name="s3" value="v3" formaction="s3.php">
</form>
```

#### 3. formmethod属性

在HTML4中，一个表单内只有一个`action`属性来对表单内的所有元素统一指定提交页面，所以每个表单内也只有一个`method`属性来统一指定提交方法。在HTML5中，可以使用`formaction`属性来对每个表单元素分别指定不同的提交页面，同时也可以使用`formmethod`属性来对每个表单元素分别指定不同的提交方法。

```html
<form id="testform" action="serve.php">
    <input type="submit" name="s1" value="v1" formaction="s1.php" formmethod="post">
    <input type="submit" name="s2" value="v2" formaction="s2.php" formmethod="get">
    <input type="submit" name="s3" value="v3" formaction="s3.php" formmethod="delete">
</form>
```

#### 4. placeholder属性

`placeholder`是指当文本框处于未输入状态时文本框中显示的输入提示。

#### 5. autofocus属性

给文本框、选择框或按钮控件加上该属性，当画面打开时，该控件自动获取光标焦点。

```html
<input type="text" autofocus>
```

#### 6. list属性

在HTML5中，为单行文本框（`<input type="text">`）增加了一个`list`属性，该属性的值为某个`datalist`元素的`id`。`datalist`元素也是HTML5中新增元素，该元素类似于选择框，但是当用户想要设定的值不在选择列表之内时，允许其自行输入。

```html
<input type="text" name="greeting" list="greetings">
<datalist id="display:none">
    <option value="Good Morning">Good Morning</option>
    <option value="Hello">Hello</option>
    <option value="Good Afternoon">Good Afternoon</option>
</datalist>
```

#### 7. autocomplete属性

辅助输入所用的自动完成功能，是一个节省输入时间，同时也十分方便的功能。在HTML5之前，因为谁都可以看见输入的值，所以存在安全隐患，但只要使用`autocomplete`属性，安全性就可以得到很好的控制。

对于`autocomplete`属性，可以指定“`on`”、“`off`”与“”这三种值。不指定时，使用浏览器的默认值。设定为“`on`”时，可以显式指定候补输入的数据列表。使用`datalist`元素与`list`属性提供候补输入的数据列表，自动完成时，可以将该`datalist`元素中的数据作为候补输入的数据在文本框中自动显示。

```html
<input type=text name=greeting autocomplete=on list=greetings>
```

---

### 二、input新增种类

在HTML5中，大幅度地增加与改良了`input`元素的种类，可以简单地使用这些元素来实现HTML5之前需要使用JavaScript才能实现的许多功能。

#### 1. search

与text文本框类似，但是它用于搜索，比如站点搜索或Google搜索

#### 2. tel

与text文本框类似，但是专用于电话

#### 3. url

与text文本框类似，但是要求用户必须在其中正确输入url格式的文字

#### 4. email

与text文本框类似，但是要求用户必须在其中正确输入email格式的文字

#### 5. datetime、date、month、week、time、datetime-local

各种日期与时间输入文本框

#### 6. number

数值输入文本框。外观与text文本框相同，但不能输入数值以外的文字

#### 7. range

只允许输入一段范围内数值的文本框。具有`min`属性和`max`属性，可以设定最小值与最大值（默认为0与100）

#### 8. color

颜色选择文本框，选择的值为`“#000000”`格式的文字

#### 9. file

文件选择文本框。与HTML4最大的不同是，可以通过指定`multiple`属性，一次选择多个文件。`value`属性的值为用逗号分割的一个或多个文件名。同时，通过把`MIME`类型指定给`accept`属性，可以限制选择文件的种类

---

### 三、表单验证

#### 1. required属性

HTML5中新增的`required`属性可以应用在大多数输入元素上，在提交时，如果元素中内容为空白，则不允许提交。

#### 2. pattern属性

对`input`元素使用`pattern`属性，并且将属性值设为某个格式的正则表达式，在提交时会检查其内容是否符合给定格式。

```html
<input pattern="[0-9][A-Z]{3}" name=part placeholder="输入内容：一个数字与三个大写字母">
```

#### 3. min和max属性

`min`、`max`属性是数值类型或日期类型的`input`元素的专用属性，它们限制了在`input`元素中输入的数值与日期的范围。

#### 4. step属性

`step`属性控制`input`元素中的值增加或减少时的步幅。

---

### 四、显式验证

除了`input`元素添加属性进行元素内容有效性的自动验证外，在HTML5中，form元素与`input`元素都具有一个`checkValidity`方法。调用该方法，可以显式地对表单内所有元素内容或单个内容进行有效性验证。

form元素与`input`元素还存在一个`validity`属性，该属性返回了一个`ValidityState`对象。该对象具有很多属性，但最简单、最重要的属性为`valid`属性，表示了表单内所有元素内容是否有效或者单个`input`元素内容是否有效。

---

### 五、取消验证

有两种方法取消表单验证：

1. 利用form元素的`novalidate`属性，它可以关闭整个表单验证。当整个表单的第二部分需要验证的内容比较多，但又想先提交表单的第一部分内容时，可以使用这种方法。先把该属性设为true，关闭表单验证，提交第一部分内容，然后在提交第二部分时再把它设为false，打开表单验证，提交第二部分内容。

2. 利用`input`元素或`submit`元素的`formnovalidate`属性，利用`input`元素的`formnovalidate`属性可以让表单验证对单个`input`元素失效。当表单的第二部分中需要验证的元素数量很少时，可以只利用这些元素的`formnovalidate`属性，让表单验证对这些元素失效。

3. 如果对`submit`按钮使用了`formnovalidate`属性，点击该按钮时，相当于利用了form元素的`novalidate`属性，整个表单验证都失效了。

---

### 六、自定义错误信息

HTML5中，可以使用Javascript调用input元素的`setCustomValidity`方法来自定义错误信息。

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()