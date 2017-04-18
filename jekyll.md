# jekyll

## 目录结构

#### _config.yml

用于保存配置 （jekyll自动加载）

> 不要在配置文件中使用`tab`制表符


#### _include文件夹

存放可以重复利用的文件，可以被其他文件包含。

> 包含的方法是：`{%include file.ext %}`

#### _layouts文件夹

存放外部模板文件。标签`{{content}}`可以将content插入页面中。

#### _posts文件夹

存放文章的地方。文件格式很重要，必须是`YEAR-MONTH-DAY-title.MARKUP`

#### _data文件夹

格式好的网站数据应该放这里。这些文件可以经由`site.data`访问。

#### _site文件夹

一旦Jekyll转换完成，就会将生成的页面放在这里。

> 其他的目录都会被拷贝到最终文件的目录下。所以，css/images等目录都可以放在根目录下。

#### _drafts

草稿，是未发布的文章。