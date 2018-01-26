---
title: H5(3) 表单
date: 2018-01-21 21:12:39
categories:
- HTML5
- 抄书笔记
---

<!-- More -->

### 一、HTML5中关于表单新增了以下属性。

#### 1. form属性

在HTML4中，表单内的从属元素必须书写在表单内部，但是在HTML5中，可以把它们书写在页面上任何地方，然后给该元素指定一个form属性，属性值为该表单的id，这样就可以声明该元素从属于指定表单了。

```html
<form id="testform">
    <input type="text">
</form>
<textarea form="testform"></textarea>
```

#### 2. formaction属性

在HTML4中，一个表单内的所有元素都只能通过表单的action属性统一提交到另一个页面，而在HTML5中可以给所有的提交按钮，诸如`<input type="submit">`、`<input type="image">`、`<button type="submit">`都增加不同的formaction属性，可以将表单提交到不同的页面。

```html
<form id="testform" action="serve.php">
    <input type="submit" name="s1" value="v1" formaction="s1.php">
    <input type="submit" name="s2" value="v2" formaction="s2.php">
    <input type="submit" name="s3" value="v3" formaction="s3.php">
</form>
```

#### 3. formmethod属性

在HTML4中，一个表单内只有一个action属性来对表单内的所有元素统一指定提交页面，所以每个表单内也只有一个method属性来统一指定提交方法。在HTML5中，可以使用formaction属性来对每个表单元素分别指定不同的提交页面，同时也可以使用formmethod属性来对每个表单元素分别指定不同的提交方法。

```html
<form id="testform" action="serve.php">
    <input type="submit" name="s1" value="v1" formaction="s1.php" formmethod="post">
    <input type="submit" name="s2" value="v2" formaction="s2.php" formmethod="get">
    <input type="submit" name="s3" value="v3" formaction="s3.php" formmethod="delete">
</form>
```

#### 4. placeholder属性

placeholder是指当文本框处于未输入状态时文本框中显示的输入提示。

#### 5. autofocus属性

给文本框、选择框或按钮控件加上该属性，当画面打开时，该控件自动获取光标焦点。

```html
<input type="text" autofocus>
```

#### 6. list属性

在HTML5中，为单行文本框（`<input type="text">`）增加了一个list属性，该属性的值为某个datalist元素的id。datalist元素也是HTML5中新增元素，该元素类似于选择框，但是当用户想要设定的值不在选择列表之内时，允许其自行输入。

```html
<input type="text" name="greeting" list="greetings">
<datalist id="display:none">
    <option value="Good Morning">Good Morning</option>
    <option value="Hello">Hello</option>
    <option value="Good Afternoon">Good Afternoon</option>
</datalist>
```

---

### 二、input新增种类

#### 1. search

与text文本框类似，但是它用于搜索

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

只允许输入一段范围内数值的文本框。

#### 8. color

颜色选择文本框，选择的值为“#000000”格式的文字。

---

### 三、表单验证

#### 1. required属性

HTML5中新增的required属性可以应用在大多数输入元素上，在提交时，如果元素中内容为空白，则不允许提交。

#### 2. pattern属性

对input元素使用pattern属性，并且将属性值设为某个格式的正则表达式，在提交时会检查其内容是否符合给定格式。

#### 3. min和max属性

min、max属性是数值类型或日期类型的input元素的专用属性，它们限制了在input元素中输入的数值与日期的范围。

#### 4. step属性

step属性控制input元素中的值增加或减少时的步幅。

---

### 四、显式验证

除了input元素添加属性进行元素内容有效性的自动验证外，在HTML5中，form元素与input元素都具有一个`checkValidity`方法。调用该方法，可以显式地对表单内所有元素内容或单个内容进行有效性验证。

form元素与input元素还存在一个validity属性，该属性反悔了一个ValidityState对象。该对象具有很多属性，但最简单、最重要的属性为valid属性，表示了表单内所有元素内容是否有效或者单个input元素内容是否有效。

---

### 五、取消验证

有两种方法取消表单验证：

1. 利用form元素的novalidate属性，它可以关闭整个表单验证。当整个表单的第二部分需要验证的内容比较多，但又想先提交表单的第一部分内容时，可以使用这种方法。先把该属性设为true，关闭表单验证，提交第一部分内容，然后在提交第二部分时再把它设为false，打开表单验证，提交第二部分内容。
2. 利用input元素或submit元素的formnovalidate属性，利用input元素的formnovalidate属性可以让表单验证对单个input元素失效。当表单的第二部分中需要验证的元素数量很少时，可以只利用这些元素的formnovalidate属性，让表单验证对这些元素失效。
3. 如果对submit按钮使用了formnovalidate属性，点击该按钮时，相当于利用了form元素的novalidate属性，整个表单验证都失效了。

---

### 六、自定义错误信息

HTML5中，可以使用Javascript调用input元素的`setCustomValidity`方法来自定义错误信息。

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()