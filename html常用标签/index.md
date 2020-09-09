# html常用标签

# HTML常用标签

## a标签
* 属性
  * href
  * target
    * 内置名字
      - _blank
      - _top
      - _parent
      - _self
      - target具体作用
          * 跳转到外部页面
          * 跳转到内部锚点
          * 跳转到邮箱或者电话
  * download
    - 不是打开页面，而是下载页面
    - 但是不是所有浏览器都支持download，手机浏览器可能不支持
  * rel = noopener



## iframe标签
内嵌窗口，已经很少使用。
在当前页面内嵌一个新窗口。

## table标签
### 表格
* 常用标签
  * table
  * thead : table head
  * tbody : table body
  * tfoot : table foot
  * tr : table row 表示表格的一行
  * th : table head 表示一行的表头
  * td : table data 表示表格内的数据  
* 相关的样式
  * table-layout : table-layout CSS属性定义了用于布局表格单元格，行和列的算法，比如设置表格的宽度等。
  * border-collapse :可以使table border之间没有间隙。
  * border-spacing : 设置table border的间隔大小。

## img标签
* 作用
  * 发出get请求，展示一张图片。
* 属性
  * alt/height/width/src
* 事件
  * onload/onerror
* 响应式
  * max-width:100%

## form标签
* 作用 
  * 发出get或者post请求，然后刷新页面
* 属性
  * action/autocomplete/method/target
* 事件
  * onsubmit

## input标签
* 作用
  * 让用户输入内容
* 属性
  * 类型type：button/checkbox/email/file/hidden/number/password/radio/search/submit/tel/text/
  * 其他 name/autofocus/checked/disabled/maxlength/pattern/value/placeholder
* 事件
  * onchange/onfocus/onblur
* 验证器
  * H5新增功能
    * require ： input不填没办法提交
  
* 其他输入标签
    * select+option
    * textarea
    * label
* 注意事项
  * 一半不见听input的click事件
  * form里面的input要有name
  * form里面要放一个type=submit才能触发submit事件。 
