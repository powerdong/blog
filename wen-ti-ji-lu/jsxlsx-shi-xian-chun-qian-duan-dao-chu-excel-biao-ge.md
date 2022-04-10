# js-xlsx实现纯前端导出excel表格

## js-xlsx

### 没有需求就没有方法

最近接到一个新需求，需求乍一看很简单，就是在价格数据表格页面增加一个数据导出功能。然而在开发过程中，遇到了这儿样那样的问题。

后端同事说他那边只支持返回 json 数据，但是对于太大的数据量他那边只能限制导出的时候进行限量。这就很尴尬，前端如何将 json 数据成功导出为用户使用的 excel 文件？

项目使用的是 [iVew](https://www.iviewui.com)UI 库。库中只提供了导出格式为 `.csv` 格式。这就很尴尬，需要自己实现。

废话不多说，来到今天的主题 **js-xlsx 实现纯前端导出 excel 表格**

### js-xlsx

[js-xlsx](https://github.com/SheetJS/js-xlsx) 是一个纯 JavaScript 实现的，能运行在所有 JavaScript 环境中，包括浏览器，NodeJs 等的 excel 库，能够读取和写入 excel 表格。

兼容性如下图： ![3llI6f.png](https://s2.ax1x.com/2020/02/23/3llI6f.png)

下面开始介绍 js-xlsx 的简单使用方式。

#### XLSX

使用之前，先安装它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script
      lang="javascript"
      src="https://unpkg.com/xlsx/dist/xlsx.full.min.js"
    ></script>
  </head>
  <body>
    <script>
      console.log(XLSX)
    </script>
  </body>
</html>
```

通过打印 `XLSX` 看看它主要有哪些属性

![3lJhsx.png](https://s2.ax1x.com/2020/02/23/3lJhsx.png)

* `SSF` 里定义了一些数据格式
* `parse` 里解析相关
* `read` 用来读取 xlsx 文件的 API
* `write` 用来把数据写入并生成 xlsx 文件的 API
* `version` 指的是 js-xlsx 的版本号

[直接跳转实现例子](https://powerdong.github.io/myBlog/2020/02/09/js-xlsx%E5%AE%9E%E7%8E%B0%E7%BA%AF%E5%89%8D%E7%AB%AF%E5%AF%BC%E5%87%BAexcel%E8%A1%A8%E6%A0%BC/#%E9%A1%B9%E7%9B%AE%E5%AE%9E%E7%8E%B0)

#### 读取 Excel 数据

为了解析数据，第一步就是读取文件，这里有几个不同的地方

* **通过文件 IO 获取数据**

```
const workBook = XLSX.readFile('test.xlsx')
```

* **因为 Excel 数据表与 HTML Table 结构是相应的，所以也可以从 Table 里获取数据**

```
// 给table标签定义id属性
const workBook = XLSX.utils.table_to_book(document.getElementById('table'))
```

![3lNvO1.png](https://s2.ax1x.com/2020/02/23/3lNvO1.png)

* **通过 url (ajax) 获取数据**

```
// 表格文件地址
let url = '/save/test.xlsx'

let req = new XMLHttpRequest()
req.open('GET', url, true)
req.responseType = 'arraybuffer'

req.onload = (s) => {
  let data = new Uint8Array(req.response)
  let workBook = XLSX.read(data, { type: 'array' })
  console.log(workBook)
}

req.send()
```

左边为输出内容，右边为表格数据

![3ldEZt.png](https://s2.ax1x.com/2020/02/23/3ldEZt.png)

* 通过 HTML5 文件拖拽获取数据

```
<div id="drop" style="width: 200px;height: 200px; background-color: aqua;">
  将文件拖到此处
</div>
<div id="excelView"></div>

<script>
  const drop = document.getElementById('drop')
  //拖进
  drop.addEventListener(
    'dragenter',
    function(e) {
      e.preventDefault()
    },
    false
  )

  //拖离
  drop.addEventListener(
    'dragleave',
    function(e) {
      dragleaveHandler(e)
    },
    false
  )

  //拖来拖去 , 一定要注意dragover事件一定要清除默认事件
  //不然会无法触发后面的drop事件
  drop.addEventListener(
    'dragover',
    function(e) {
      e.preventDefault()
    },
    false
  )

  //扔
  drop.addEventListener(
    'drop',
    function(e) {
      dropHandler(e)
    },
    false
  )

  console.log(drop)
  const excelView = document.getElementById('excelView')

  drop.addEventListener('drop', function(e) {
    console.log(11)
    // e.stopPropagation();
    e.preventDefault()
    let files = e.dataTransfer.files
    console.log(files)
    let reader = new FileReader()
    reader.readAsArrayBuffer(files[0])
    reader.onload = function(event) {
      let data = event.target.result
      let wb = XLSX.read(data, { type: 'array' })

      var wsName = wb.SheetNames[0]
      var ws = wb.Sheets[wsName]

      // 渲染
      excelView.innerHTML = XLSX.utils.sheet_to_html(ws)
    }
  })
</script>
```

![3lDx3T.gif](https://s2.ax1x.com/2020/02/23/3lDx3T.gif)

HTML5 拖拽上传文件请[点击](https://powerdong.github.io/myBlog/2019/07/14/HTML5-%E6%8B%96%E6%8B%BD%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6/)查看

js-xlsx 的解析数据 API

| 函数                                  | 描述           |
| ----------------------------------- | ------------ |
| XLSX.read(data, read\_opts)         | 尝试解析 data    |
| XLSX.readFile(filename, read\_opts) | 根据文件路径名，继续解析 |

**workBook**

`workBook` 就是读取 Excel 数据后的 json 对象，里面记录着 Excel 的数据信息

![3lsqf0.png](https://s2.ax1x.com/2020/02/23/3lsqf0.png)

* `Custprops` 存储自定义属性
* `Workbook` 存储工作簿属性
* `SSF` 定义一些数据格式
* `Directory` 是 Excel 文件的描述对象
* `Props` 是 xlsx 文件的属性
* `SheetNames` 里存储着每个 Sheet 的名字
* `Sheets` 里面存储的是每个 Sheet 的数据

每个 Sheet 对象里都根据坐标存储 Excel 的二维数据，其中 `!ref` 代表这个表的范围

![3l6yGt.png](https://s2.ax1x.com/2020/02/23/3l6yGt.png)

对应的是 Excel 表数据

![3l6Wqg.png](https://s2.ax1x.com/2020/02/23/3l6Wqg.png)

其中 Strings 是一个数组，他按照从左到右，从上到下的顺序来把**二维**的数据降到**一维**

```
const arr = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]

const cat = (...args) => arg.reduce((acc, cur) => [...acc, ...cur], [])

// 将二维数组降到一维
cat(...arr) // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**单元格对象**

在下图中 A1 就是一个单元格对象，它代表着 Excel 二维表里的 A1 位置的单元格对象。

![3lcQQf.png](https://s2.ax1x.com/2020/02/23/3lcQQf.png)

| 属性 | 描述                                    |
| -- | ------------------------------------- |
| v  | 原始值                                   |
| w  | 格式化文本                                 |
| t  | 单元格类型：b 布尔值，n 数字，e 错误，s 字符串，d 日期      |
| f  | 单元格的公式， 例如 `f: 'A1 + A2'`             |
| F  | 如果公式是数组公式                             |
| r  | 富文本编码                                 |
| h  | 富文本的 HTML 呈现                          |
| c  | 与单元格相关的注释                             |
| z  | 与单元格关联的数字格式字符串                        |
| l  | 单元格超链接对象（.Target 持有链接，.Tooltip 是工具提示） |
| s  | 单元格的样式/主题（可以设置字体颜色，加粗等）               |

#### 导出数据到文件

对于写入，第一步是生成输出数据。对于生成输出数据也分为几种。

* 在 NodeJs 上

```
const XLSX = require('xlsx')
XLSX.writeFile(workBook, 'out.xlsx')
```

* 因为 Excel 是一个表格结构，因此可以轻易输出 HTML Table

```
const workSheet = workBook.Sheets[workBook.SheetNames[0]]
const container = document.getElementById('table')
container.innerHTML = XLSX.utils.sheet_to_html(workSheet)
```

* 浏览器保存

```
// bookType can be 'xlsx' or 'xlsm' or 'xlsb'
let wOpts = { bookType: 'xlsx', bookSST: false, type: 'binary' }
let wBout = XLSX.write(workBook, wOpts)

/**
 * 字符串转字符流
 */

function s2ab(s) {
  var buf = new ArrayBuffer(s.length)
  var view = new Unit8Array(buf)
  for (let i = 0; i != s.length; ++i) {
    view[i] = s.charCodeAt(i) & 0xff
  }
  return buf
}
// saveAs调用在本地计算机上下载文件
saveAs(
  new Blob([s2ab(wBout)], { type: 'application/octet-stream' }),
  'test.xlsx'
)
```

* 上传到服务器

```
// 通过 FormData 上传到服务器
let wOpts = { bookType: 'xlsx', bookSST: false, type: 'base64' }
let wBout = XLSX.write(workbook, wOpts)

let req = new XMLHttpRequest()
req.open('POST', '/upload', true)
let formData = new FormData()
formData.append('file', 'test.xlsx')
formData.append('data', wBout)
req.send(formData)
```

**`js-xlsx` 写入数据 API**

| 函数                                        | 描述         |
| ----------------------------------------- | ---------- |
| XLSX.write(wb, write\_opts)               | 写到 wb 里    |
| XLSX.writeFIle(wb, filename, write\_opts) | 写入文件里      |
| XLSX.writeFileAsync(filename,wb,o,cb)     | 异步方式，写入文件里 |

#### 工具函数

`XLSX.utils` 是一个工具 API，一些函数用于把数据导出各种格式

`sheet_to_*` 函数接受一个工作表和一个可选的选项对象

`*_to_sheet` 函数接受一个数据对象和一个可选选项对象

| 属性/函数                                          | 描述                           |
| ---------------------------------------------- | ---------------------------- |
| XLSX.utils.sheet\_to\_csv                      | 生成 CSV。                      |
| XLSX.utils.sheet\_to\_html                     | 生成 HTML。                     |
| XLSX.utils.sheet\_to\_json                     | 生成一个对象数组。                    |
| XLSX.utils.sheet\_to\_formulae                 | 生成公式列表。                      |
| XLSX.utils.book\_new()                         | 创建一个默认的 Excel 文件描述对象。        |
| XLSX.utils.book\_append\_sheet(wb, ws, “test”) | 向 workbook 添加一个 ws，名称为 test。 |
| XLSX.utils.aoa\_to\_sheet                      | 将 js 数据的数组转换为工作表。            |
| XLSX.utils.json\_to\_sheet                     | 将 js 对象的数组转换为工作表。            |
| XLSX.utils.table\_to\_sheet                    | 将 DOM TABLE 元素转换为工作表。        |
| XLSX.utils.sheet\_to\_json                     | 将工作表对象转换为 JSON 对象数组。         |
| XLSX.utils.sheet\_to\_csv                      | 生成分隔符分隔值输出。                  |
| XLSX.utils.sheet\_to\_html                     | 生成 HTML 输出。                  |
| XLSX.utils.sheet\_to\_formulae                 | 生成公式列表（具有值回退）。               |
| XLSX.utils.format\_cell                        | 生成单元格的文本值（使用数字格式）。           |
| XLSX.utils.encode\_row/decode\_row             | 在 0 索引行和 1 索引行之间进行转换。        |
| XLSX.utils.encode\_col/decode\_col             | 在 0 索引列和列名之间进行转换。            |
| XLSX.utils.encode\_cell/decode\_cell           | 转换单元格地址。                     |
| XLSX.utils.encode\_range/decode\_range         | 转换单元格范围。                     |

#### 数据自定义

下面介绍几种方式来定义数据对象

* 整列数组
* 对象数组
* HTML Table

**阵列数组**

```
// 构造一个二维表
const ws = XLSX.utils.aoa_to_sheet(['我的名字是Lambda'.split('')])

/**
    {
    A1: {v: "我", t: "s"}
    B1: {v: "的", t: "s"}
    C1: {v: "名", t: "s"}
    D1: {v: "字", t: "s"}
    E1: {v: "是", t: "s"}
    F1: {v: "L", t: "s"}
    G1: {v: "a", t: "s"}
    H1: {v: "m", t: "s"}
    I1: {v: "b", t: "s"}
    J1: {v: "d", t: "s"}
    K1: {v: "a", t: "s"}
    !ref: "A1:K1"
    }
 */
```

**对象数组**

```
const ws = XLSX.utils.json_to_sheet(
  [
    { S: 1, h: 2, e: 3, e_1: 4, t: 5, J: 6, S_1: 7 }, // 第一行
    { S: 2, h: 3, e: 4, e_1: 5, t: 6, J: 7, S_1: 8 } // 第二行
  ],
  {
    header: ['S', 'h', 'e', 'e_1', 't', 'J', 'S_1'] // 表头
  }
)
```

![319Gx1.png](https://s2.ax1x.com/2020/02/23/319Gx1.png)

**HTML Table**

```
<table id="sheet">
  <tr>
    <td>S</td>
    <td>h</td>
    <td>e</td>
    <td>e</td>
    <td>t</td>
    <td>J</td>
    <td>S</td>
  </tr>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
  </tr>
  <tr>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
  </tr>
</table>

<script>
  const tbl = document.getElementById('sheet')
  const wb = XLSX.utils.table_to_book(tbl)

  // 之后需要将新的数据对象添加到workBook里
  let ws_name = 'Sheet'
  let ws = XLSX.utils.aoa_to_sheet([
    ['S', 'h', 'e', 'e', 't', 'J', 'S'],
    [1, 2, 3, 4, 5]
  ])

  wb.SheetNames.push(ws_name)
  wb.Sheets[ws_name] = ws
</script>
```

#### 生成 xlsx 文件

js-xlsx 提供一个默认的 `workBook` 对象(XLSX.utils.book\_new)

```
const ws = XLSX.utils.json_to_sheet(
  [
    { name: 'Lambda', age: 21, sex: '男' }, // 第一行
    { name: 'Lambda1', age: 21, sex: '男' }, // 第一行
    { name: 'Lambda2', age: 21, sex: '男' } // 第一行
  ],
  {
    header: ['name', 'age', 'sex'] // 表头
  }
)

var tmpWB = {
  SheetNames: ['sheet'], //保存的表标题
  Sheets: {
    sheet: Object.assign(
      {},
      ws, //内容
      {}
    )
  }
}

tmpDown = new Blob(
  [
    s2ab(
      XLSX.write(
        tmpWB,
        {
          // bookType can be 'xlsx' or 'xlsm' or 'xlsb'
          bookType: type == undefined ? 'xlsx' : type,
          bookSST: false, // 是否生成Shared String Table，官方解释是，如果开启生成速度会下降，但在低版本IOS设备上有更好的兼容性
          type: 'binary'
        } //这里的数据是用来定义导出的格式类型
      )
    )
  ],
  {
    type: ''
  }
) //创建二进制对象写入转换好的字节流

const outFile = document.createElement('a')
var href = URL.createObjectURL(tmpDown) // 创建对象超链接
outFile.download = '下载名称.xlsx' // 下载名称
outFile.style.display = 'none'
outFile.href = href // 绑定a标签
document.body.appendChild(outFile)
outFile.click() // 模拟点击实现下载
setTimeout(function() {
  // 延时释放
  URL.revokeObjectURL(tmpDown) // 用URL.revokeObjectURL()来释放这个object URL
  document.body.removeChild(outFile)
}, 100)

// 字符串转字符流
function s2ab(s) {
  var buf = new ArrayBuffer(s.length)
  var view = new Uint8Array(buf)
  for (var i = 0; i != s.length; ++i) view[i] = s.charCodeAt(i) & 0xff
  return buf
}
```

**兄弟们，实现导出了！！**

![31ksD1.png](https://s2.ax1x.com/2020/02/23/31ksD1.png)

#### 合并单元格功能

合并表格的数据在 Sheets Object 里的 !merges 属性里，它是一个数组，数组的顺序是按照二维表顺序（从左到右，从上到下）排列。

![31nSr4.png](https://s2.ax1x.com/2020/02/23/31nSr4.png)

```
;[
  {
    s: { c: 1, r: 1 }, // 第 2 列第 2 行开始，也就是 B2
    e: { c: 1, r: 3 } // 第 2 列第 4 行结束，也就是 B4
  }
]
```

> 其中 s 是 start 的简写，e 是 end 的简写，c 是 column 的简写，r 是 row 的简写（这种简写方式主要是为了节约内存，想想 Excel 表里可能有上万个单元格，如果用全称实在是太浪费内存）。

```
const ws = XLSX.utils.json_to_sheet(
  [
    { name: 'Lambda', age: 21, sex: '男' }, // 第一行
    { name: 'Lambda1', age: 21, sex: '男' }, // 第一行
    { name: 'Lambda2', age: 21, sex: '男' } // 第一行
  ],
  {
    header: ['name', 'age', 'sex'] // 表头
  }
)
ws['!merges'] = [
  {
    s: { c: 1, r: 1 }, // 第 2 列第 2 行开始，也就是 B2
    e: { c: 1, r: 3 } // 第 2 列第 4 行结束，也就是 B4
  }
]
```

### 项目实现

#### 导入

```
<script src="https://unpkg.com/xlsx/dist/xlsx.full.min.js"></script>

<input type="file" onchange="importFIle(this)" />
<div id="demo"></div>
<script>
  /*
   FileReader共有4种读取方法：
   1.readAsArrayBuffer(file)：将文件读取为ArrayBuffer。
   2.readAsBinaryString(file)：将文件读取为二进制字符串
   3.readAsDataURL(file)：将文件读取为Data URL
   4.readAsText(file, [encoding])：将文件读取为文本，encoding缺省值为'UTF-8'
    */
  var wb //读取完成的数据
  var rABS = false //是否将文件读取为二进制字符串

  function importFIle(obj) {
    //导入
    if (!obj.files) {
      return
    }
    var f = obj.files[0]
    var reader = new FileReader()
    reader.onload = function(e) {
      var data = e.target.result
      if (rABS) {
        wb = XLSX.read(btoa(changeData(data)), {
          //手动转化
          type: 'base64'
        })
      } else {
        wb = XLSX.read(data, {
          type: 'binary'
        })
      }
      //wb.SheetNames[0]是获取Sheets中第一个Sheet的名字
      //wb.Sheets[Sheet名]获取第一个Sheet的数据
      const showData = JSON.stringify(
        XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]])
      )
      //  这里可以根据需要添加转换条件
      document.getElementById('demo').innerHTML = showData
    }
    if (rABS) {
      reader.readAsArrayBuffer(f)
    } else {
      reader.readAsBinaryString(f)
    }
  }

  function changeData(data) {
    //文件流转BinaryString
    var o = '',
      l = 0,
      w = 10240
    for (; l < data.byteLength / w; ++l)
      o += String.fromCharCode.apply(
        null,
        new Uint8Array(data.slice(l * w, l * w + w))
      )
    o += String.fromCharCode.apply(null, new Uint8Array(data.slice(l * w)))
    return o
  }
</script>
```

![31u4BQ.png](https://s2.ax1x.com/2020/02/23/31u4BQ.png)

#### 导出

```
<script src="https://unpkg.com/xlsx/dist/xlsx.full.min.js"></script>


<button onclick="exportData()">导出</button>

<script>
  let isExport = false
  /**
   * 点击导出按钮，请求数据
   */
  function exportData() {
    // 自定义文件名
    const fileName = setFileName(),
      // 正在导出
      isExport = true
    // 请求数据
    this.$store
      .dispatch('')
      .then((res) => {
        if (res.code == 200) {
          const resData = res.data
          let data = [{}]
          for (let k in res.data[0]) {
            switch (k) {
              // json 数据属性名
              case 'id':
                data[0][k] = '编号'
                break
            }
          }
          data = data.concat(resData)
          downloadExl(data, fileName)
          isExport = false
        } else {
          isExport = false
        }
      })
      .catch((err) => {
        if (err) {
          isExport = false
        }
      })
  }
  /**
   * 导出到Excel
   * @param {*} json 导出内容
   * @param {*} fileName 文件名称
   * @param {*} type 文件类型
   */
  function downloadExl(json, fileName, type) {
    let keyMap = [] // 获取键
    for (let k in json[0]) {
      keyMap.push(k)
    }
    let tmpData = [] // 用来保存转换好的json
    json
      .map((v, i) =>
        keyMap.map((k, j) =>
          Object.assign(
            {},
            {
              v: v[k],
              position:
                (j > 25 ? this.getCharCol(j) : String.fromCharCode(65 + j)) +
                (i + 1)
            }
          )
        )
      )
      .reduce((prev, next) => prev.concat(next))
      .forEach(function(v) {
        tmpData[v.position] = {
          v: v.v
        }
      })
    let outputPos = Object.keys(tmpData) // 设置区域,比如表格从A1到D10
    let tmpWB = {
      SheetNames: ['sheet'], // 保存的表标题
      Sheets: {
        sheet: Object.assign(
          {},
          tmpData, // 内容
          {
            '!ref': outputPos[0] + ':' + outputPos[outputPos.length - 1] // 设置填充区域
          }
        )
      }
    }
    let tmpDown = new Blob(
      [
        this.s2ab(
          XLSX.write(
            tmpWB,
            {
              bookType: type === undefined ? 'xlsx' : type,
              bookSST: false,
              type: 'binary'
            } // 这里的数据是用来定义导出的格式类型
          )
        )
      ],
      {
        type: ''
      }
    ) // 创建二进制对象写入转换好的字节流
    const outFile = document.createElement('a')
    var href = URL.createObjectURL(tmpDown) // 创建对象超链接
    outFile.download = fileName + '.xlsx' // 下载名称
    outFile.style.display = 'none'
    outFile.href = href // 绑定a标签
    document.body.appendChild(outFile)
    outFile.click() // 模拟点击实现下载
    setTimeout(function() {
      // 延时释放
      URL.revokeObjectURL(tmpDown) // 用URL.revokeObjectURL()来释放这个object URL
      document.body.removeChild(outFile)
    }, 100)
  }
  /**
   * 字符串转字符流
   * @param {String} s
   */
  function s2ab(s) {
    var buf = new ArrayBuffer(s.length)
    var view = new Uint8Array(buf)
    for (var i = 0; i !== s.length; ++i) {
      view[i] = s.charCodeAt(i) & 0xff
    }
    return buf
  }
  /**
   * 将指定的自然数转换为26进制表示。映射关系：[0-25] -> [A-Z]。
   * @param {Number} n
   */
  function getCharCol(n) {
    let s = ''
    let m = 0
    while (n > 0) {
      m = (n % 26) + 1
      s = String.fromCharCode(m + 64) + s
      n = (n - m) / 26
    }
    return s
  }
  // 设置文件名  eg: 2020-02-02
  function setFileName() {
    let str = ''
    const date = new Date()
    str += `${date.getFullYear()}-${
      date.getMonth() + 1 > 10 ? date.getMonth() + 1 : '0' + date.getMonth()
    }-${date.getDate() > 10 ? date.getDate() : '0' + date.getDate()}`
    return str
  }
</script>
```
