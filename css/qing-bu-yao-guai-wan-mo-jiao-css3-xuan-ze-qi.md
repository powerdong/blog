# 请不要拐弯抹角 —— CSS3 选择器

### Css3 选择器

CSS 选择器用于选择你想要的元素的样式的模式。

#### 属性选择器

**语法**

* `[target=-blank]` 选择所有使用`target="-blank"`的元素
* `[title~=flower]` 选择标题属性包含单词”flower”的所有元素
* `[lang|=en]` 选择 lang 属性以 en 为开头的所有元素 (**以单词开头**)
* `a[src^="https"]` 选择每一个 src 属性的值以”https”开头的元素(**以字母开头**)
* `a[src$=".pdf"]` 选择每一个 src 属性的值以”.pdf”结尾的元素
* `a[src*="run"]` 选择每一个 src 属性的值包含子字符串”run”的元素

**代码示例**

```
<p class="gray">[attribute=value]选择匹配属性值的每个元素设置样式</p>
<p class="include red">[attribute~=value]选择标题属性包含指定值的所有元素</p>
<p class="blue-front have">
  [attribute|=value]选择器用于选取带有以指定值开头的属性值的元素。
</p>
<p class="green have front">
  [attribute^=value]选择器匹配属性值以指定值开头的每个元素。
</p>
<p class="yellow bank">
  [attribute$=value]选择器匹配属性值以指定值结尾的每个元素。
</p>
<p class="have bank">
  [attribute*=value]选择器匹配属性值存在指定值的每个元素。
</p>
p[class='gray'] {
  background: gray;
}

p[class~='red'] {
  background: red;
}

p[class|='blue'] {
  background: blue;
}

p[class^='gre'] {
  background: green;
}

p[class$='ank'] {
  background: yellow;
}

p[class*='have'] {
  font-size: 18px;
  font-weight: 700;
}
```

![mKXOaT.png](https://s2.ax1x.com/2019/08/18/mKXOaT.png)

### 伪类与伪元素的区别

伪类的效果可以通过添加一个实际的类来达到 伪元素的效果则需要通过添加一个实际的元素才能达到

#### 伪类选择器

* `:root`: 选择文档的根元素，等同于`<html>`元素,建议使用 `:root`
* `:not(p)`: 选择每个并非 p 元素的元素,用法和 jQuery 中的 not 类似，可以排除某些特定条件的元素
* `:empty` : 选择每个没有任何内容的元素、不在文档树中的元素（包括文本节点）
* `:target` : 选择当前活动的元素,锚点选择器（用来匹配被 `location.hash` 选中的元素）
* 均有弊端，即如果当前位置元素不是前面所修饰的元素，那么无效
  * `:first-child` : 指定只有当某元素是其父级的第一个子级的样式。
  * `last-child` : 选择某元素是其父级的最后一个子级。
  * `:nth-child(n)` : 选择第 n 个子元素，n 代表变量自然数(可填表达式 2n+1)
  * `:nth-last-child(n)` : 从后往前数，选择第 n 个子元素，n 代表变量自然数(可填表达式 2n+1)
* 限制了类型，即在所修饰元素的类型下选择特定位置的元素
  * `:first-of-type` : 第一个子元素
  * `:last-of-type` : 最后一个子元素
  * `:nth-of-type(n)` : 第 n 个子元素，n 代表变量自然数
  * `:nth-last-of-type(n)` : 从后往前数，第 n 个子元素，n 代表变量自然数
*   ```
    :only-child
    ```

    : 唯一子元素选择器，选择是独生子的子元素，即该子元素不能有兄弟元素，它的父级只有他一个直接子元素

    * \*\*注意：\*\*选择的元素是独生子子元素，而非唯一子元素的父元素
* `:only-of-type` : 如果要选择第某类特定的子元素(p)在兄弟节点中是此类元素唯一个的话，就需要用到这个属性了
*   在 Web 表单中，有些表单元素有可用(‘enabled’)和不可用(‘disabled’)状态，比如输入框，密码框，复选框等。在默认情况下，这些表单元素都处在可用状态。那么我们可以通过伪类元素器

    ```
    enabled
    ```

    进行选择，

    ```
    disabled
    ```

    则相反。

    * `:enabled` : 可用的元素
    * `:disabled` : 不可用的元素
*   ```
    :checked
    ```

    : 选择框的被选中状态

    * \*\*注：\*\*checkbox，radio 的一些默认状态不可用属性进行改变，如边框颜色
*   ```
    :read-only
    ```

    : 选中只读的元素

    * `<input type="text" readonly="readonly"/>`
* `:read-write` : 选中非只读的元素

**代码示例**

```
<h1>这是一个标题</h1>
<div>
  <p>这是第一个段落。</p>
  <p>
    这是第二个段落。
    <span>这是个独生子</span>
  </p>
  <p>这是第三个段落。</p>
  <p>这是第四个段落。</p>
</div>
<div></div>
<input type="text" disabled="disabled" value="Disneyland" /><br />
<input type="text" value="Enabled" /><br />
<input type="radio" checked="checked" value="male" name="gender" /> Male<br />
<input type="checkbox" checked="checked" value="Bike" /> I have a bike<br />
<input type="text" readonly value="Readonly" />
:root {
  background: #ccc;
}
div:empty {
  width: 20px;
  height: 20px;
  background: #000;
}
p:first-child {
  /*  子元素中的第一个  */
  color: red;
}
p:last-of-type {
  color: green;
}
span:only-child {
  color: blue;
}
input:enabled {
  border: 3px solid;
}
input:disabled {
  border: 5px solid red;
}
input:checked {
  height: 30px;
  width: 30px;
}
input:read-only {
  width: 100px;
  height: 20px;
}
input:read-write {
  background: #999;
}
```

![mMZJYj.png](https://s2.ax1x.com/2019/08/18/mMZJYj.png)

#### 伪元素选择器

* `::before` : 在元素之前插入内容
* `::after` : 在元素之后插入内容
* `::first-line` : 第一行
* `::first-letter` : 第一个字母
*   ```
    ::selection
    ```

    : 匹配元素中被用户选中或处于高亮状态的部分

    * 火狐浏览器必须加 `-moz-` (`-moz-::selection`)

**代码示例**

```
<h1>这是一个标题</h1>
<div>
  <p>这是第一个段落。</p>
  <p>这是第二个段落。</p>
  <p>这是第三个段落。</p>
  <p>这是第四个段落。</p>
</div>
div::before {
  content: '头部插入';
}

div::after {
  content: '尾部插入';
}

div::first-line {
  color: red;
}

h1::first-letter {
  font-size: 50px;
}

p::selection {
  background: yellow;
}
```

![mMMC6A.png](https://s2.ax1x.com/2019/08/18/mMMC6A.png)

#### 两者的区别

1. CSS 伪类用于向某些选择器添加特殊的效果 CSS 伪元素用于将特殊的效果添加到某些选择器
2. 伪类的效果可以通过添加一个实际的类来达到 伪元素的效果则需要通过添加一个实际的元素才能达到
3. CSS3 明确规定伪类用一个冒号(‘`:`‘)来表示 伪元素用两个冒号(‘`::`‘)来表示

#### 条件选择器

* `>` : 直接子元素
* `+` : 紧挨着的兄弟节点
* `~` : 后面的兄弟节点

**代码示例**

```
<h1>这是一个标题</h1>
<div>
  <p>这是第一个段落。</p>
  <p>这是第二个段落。</p>
  <p>这是第三个段落。</p>
  <p>这是第四个段落。</p>
  <p>这是第五个段落。</p>
  <p>这是第六个段落。</p>
</div>
div > p {
  color: red;
}
p:first-child + p {
  color: blue;
}

p:nth-of-type(3) ~ p {
  color: green;
}
```

![mMQkv9.p](https://s2.ax1x.com/2019/08/18/mMQkv9.png)
