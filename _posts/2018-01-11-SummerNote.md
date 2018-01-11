---
layout: post
title: SummerNote
key: fxHqCAcTXtnRTXu2Uf3Efgo6
tags: model
---

# SummerNote富文本插件的使用
[summernote https://summernote.org/](https://summernote.org/) <br>
[github网址](https://github.com/summernote/summernote)<br>

### 快速使用

1. include JS/CSS

```
<!-- include libraries(jQuery, bootstrap) -->
<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script> 
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.5/css/bootstrap.min.css" />
<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.5/js/bootstrap.min.js"></script>

<!-- include summernote css/js-->
<link href="summernote.css" rel="stylesheet">
<script src="summernote.js"></script>
```

2. 定义一个来显示富文本的区域
```
<div class="summernote"></div>
```

3. 在js代码中初始化区域

```
$(function(
	//使用summernote方法初始化
	$(".summernote").summernote();
	//初始化时可以指定高度等属性
	$(".summernote").summernote({
		height: 350, 
		codemirror: { // codemirror options
			theme: 'monokai'
		}
	});
	
));

```

*这样基本就可以看到效果了*
<hr>
4. 使用summernote('code')获取文本内容
```
var context = $(".summernote").summernote('code');
```

5. 获取焦点
```
$('.summernot').summernote({
	focus: true
});
```

**上传插件的功能？**






