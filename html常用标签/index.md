# html常用标签


# HTML 常用标签

## a 标签

属性

- href
- target
  - \_blank
  - \_top
  - \_parent
  - \_self
  - target
    - 跳转到外部页面
    - 跳转到内部锚点
    - 跳转到邮箱或者电话
  - download
    - 不是打开页面，而是下载页面
    - 但是不是所有浏览器都支持 download，手机浏览器可能不支持
  - rel = noopener

## iframe 标签

内嵌窗口，已经很少使用。
在当前页面内嵌一个新窗口。

## table 标签

### 表格

- 常用标签
  - table
  - thead : table head
  - tbody : table body
  - tfoot : table foot
  - tr : table row 表示表格的一行
  - th : table head 表示一行的表头
  - td : table data 表示表格内的数据
- 相关的样式
  - table-layout : table-layout CSS 属性定义了用于布局表格单元格，行和列的算法，比如设置表格的宽度等。
  - border-collapse :可以使 table border 之间没有间隙。
  - border-spacing : 设置 table border 的间隔大小。

## img 标签

- 作用
  - 发出 get 请求，展示一张图片。
- 属性
  - alt/height/width/src
- 事件
  - onload/onerror
- 响应式
  - max-width:100%

## form 标签

- 作用
  - 发出 get 或者 post 请求，然后刷新页面
- 属性
  - action/autocomplete/method/target
- 事件
  - onsubmit

## input 标签

- 作用
  - 让用户输入内容
- 属性
  - 类型 type：button/checkbox/email/file/hidden/number/password/radio/search/submit/tel/text/
  - 其他 name/autofocus/checked/disabled/maxlength/pattern/value/placeholder
- 事件
  - onchange/onfocus/onblur
- 验证器
  - H5 新增功能
    - require ： input 不填没办法提交
- 其他输入标签
  - select+option
  - textarea
  - label
- 注意事项
  - 一半不见听 input 的 click 事件
  - form 里面的 input 要有 name
  - form 里面要放一个 type=submit 才能触发 submit 事件。

