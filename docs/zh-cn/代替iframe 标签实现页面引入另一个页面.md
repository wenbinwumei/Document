# 代替iframe 标签实现页面引入另一个页面

```js
var src="引入页面的url"; 
$.get(src,function(data){ 
	$("当前页面要显示引入页面的id或者class").html(data); 
});
```

iframe虽然好用，但是其弊端也很明显，一是它不能使用于响应式布局，iframe的使用必须指定高度，而响应式布局的高度兵分固定的。其次iframe不易被搜索引擎的爬虫解读，特别是iframe中嵌套iframe，这是会被搜索引擎认为是个死网站而被放过。

目前主流的应用都使用了ajax代替了iframe。



`<``ul` `class``=``"nav navbar-nav"` `id``=``"indexMenu"``>`` ``<``li``><``a` `target``=``"main/main.html"``>首页</``a``></``li``>`` ``<``li``><``a` `target``=``"new/new.html"``>新闻</``a``></``li``>`` ``<``li``><``a` `target``=``"leave/leave.html"``>留言</``a``></``li``>`` ``<``li``><``a` `target``=``"download/download.html"``>资料下载</``a``></``li``>``</``ul``>`



```html
<div id="iframeContent">content</div>
```

`$(``function``(){`` ``$.get(``"main/main.html"``,``function``(data){`` ``$(``"#iframeContent"``).html(data);``//初始化加载界面`` ``});`` ` ` ``$(``'#indexMenu li'``).click(``function``(){``//点击li加载界面`` ``var` `current = $(``this``),`` ``target = current.find(``'a'``).attr(``'target'``); ``// 找到链接a中的targer的值`` ``$.get(target,``function``(data){``  ``$(``"#iframeContent"``).html(data); ``  ``});`` ``});``});`

```javascript
$(function(){
 	$.get("main/main.html",function(data){
 		$("#iframeContent").html(data);//初始化加载界面
	});

    $('#indexMenu li').click(function(){//点击li加载界面
        var current = $(this),
        target = current.find('a').attr('target'); // 找到链接a中的targer的值
        $.get(target,function(data){
            $("#iframeContent").html(data); 
        });
    });
});
```

这样做不仅满足了响应式布局，而且div也能被爬虫认识，故而更受欢迎！