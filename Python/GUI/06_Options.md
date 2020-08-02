## Options

常见的选项汇总

| 选项                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| activebackground    | 指定组件处于激活状态时的背景色                               |
| activeforeground    | 指定组件处于激活状态时的前景色                               |
| anchor              | 指定组件内的信息（比如文本或图片） 在组件中如何显示(当所在组件比信 息大时， 可以看出效果)。 必须为下面的值之一： N、 NE、 E、 SE、 S、 SW W、 NW 或 CENTER。 比如 NW（NorthWest） 指定将信息显示在组件的左上角 |
| background(bg)      | 指定组件正常显示时的背景色                                   |
| bitmap              | 指定在组件上显示该选项指定的位图                             |
| borderwidth         | 指定组件正常显示时的 3D 边框的宽度， 该值可以是 Tk_GetPixels 接收的任 何格式 |
| cursor              | 指定光标在组件上的样式。 该值可以是 Tk_GetCursors 接受的任何格式 |
| command             | 指定按组件关联的命令方法， 该方法通常在鼠标离开组件时被触发调用 |
| disabledforeground  | 指定组件处于禁用状态时的前景色                               |
| font                | 指定组件上显示的文本字体                                     |
| foreground(fg)      | 指定组件正常显示时的前景色                                   |
| highlightbackground | 指定组件在高亮状态下的背景色                                 |
| highlightcolor      | 指定组件在高亮状态下的前景色                                 |
| highlightthickness  | 指定组件在高亮状态下的周围方形区域的宽度， 该值可以是 Tk_GetPixels 接收的任何格式 |
| height              | 指定组件的高度， 以 font 选项指定的字体的字符高度为单位， 至少为 1 |
| image               | 指定组件中显示的图像， 如果设置了 image 选项， 它将会覆盖 text、 bitmap 选项 |
| justify             | 指定组件内部内容的对齐方式， 该选项支持 LEFT（左对齐） 、 CENTER（居 中对齐） 或 RIGHT（右对齐） 这三个值 |
| padx                | 指定组件内部在水平方向上两边的空白， 该值可以是 Tk_GctPixels 接收的 任何格式 |
| pady                | 指定组件内部在垂直方向上两地的空白， 该值可以是 Tk_GctPixels 接收的 任何格式 |
| relief              | 指定组件的 3D 效果， 该选项支持的值包括 RAISED、 SUNKEN、 FLAT、 RIDGE、 SOLID、 GROOVE。 该值指出组件内部相对于外部的外观样式， 比如 RAISED 表示组件内部相对于外部凸起 |
| selectbackground    | 指定组件在选中状态下的背景色                                 |
| selectborderwidth   | 指定组件在选中状态下的 3D 边框的宽度， 该值可以是 Tk_GetPixels 接收的 任何格式 |
| selectforeground    | 指定组在选中状态下的前景色                                   |
| state               | 指定组件的当前状态。 该选项支持 NORMAL（正常） 、 DISABLE（禁用） 这两个值 |
| takefocus           | 指定组件在键盘遍历（Tab 或 Shift+Tab） 时是否接收焦点， 将该选项设为 1 表示接收焦点； 设为 0 表示不接收焦点 |
| text                | 指定组件上显示的文本， 文本显示格式由组件本身、 anchor 及 justify 选 项决定 |
| textvariable        | 指定一个变量名， GUI 组件负责显示该变量值转换得到的字符串， 文本显 示格式由组件本身、 anchor 及 justify 选项决定 |
| underline           | 指定为组件文本的第几个字符添加下画线， 该选项就相当于为组件绑定了 快捷键 |
| width               | 指定组件的宽度， 以 font 选项指定的字体的字符高度为单位， 至少为 1 |
| wraplength          | 对于能支持字符换行的组件， 该选项指定每行显示的最大字符数， 超过该数量的字符将会转到下行显示 |
| xscrollcommand      | 通常用于将组件的水平滚动改变（包括内容滚动或宽度发生改变） 与水平 滚动条的 set 方法关联， 从而让组件的水平滚动改变传递到水平滚动条 |
| yscrollcommand      | 通常用于将组件的垂直滚动改变（包括内容滚动或高度发生改变） 与垂直 滚动条的 set 方法关联， 从而让组件的垂直滚动改变传递到垂直滚动条 |

