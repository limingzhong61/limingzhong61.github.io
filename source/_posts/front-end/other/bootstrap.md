# bootstrap4

## Justify content（class实现flex布局）

https://v4.bootcss.com/docs/4.3/utilities/flex/#justify-content

# bootstrap4 调整元素之间距离

https://v4.bootcss.com/docs/4.3/utilities/spacing/

https://blog.csdn.net/jianye5461/article/details/79463014

py ===== p-d+padding-top

mb ===== margin-bottom

# bootstrap气泡效果

https://v4.bootcss.com/docs/4.3/components/popovers/

click. focus 触发

# bootstrap实现

## 栅格系统 gird System

js实现

```javascript
<script type="text/javascript">
	window.onload = function(){
		var box = document.querySelector(".box");
		resize();
		window.addEventListener("resize",resize);
		function resize(){
			// 2.1 获取改变后的宽度
			var clientW = window.innerWidth;			
			// 2.2 判断
			if(clientW >= 1200){ // 超大屏幕
				box.style.width = "1170px";
			}else if(clientW >= 992){ // 大屏幕
				box.style.width = "970px";
			}else if(clientW >= 768){ // 小屏幕
				box.style.width = "750px";
			}else { // 超小屏幕
				box.style.width = "100%";
			}
		}
	}
</script>
```

@media 媒体查询方式（bootstrap实现方式）

```css
<style type="text/css">
*{
    margin: 0;
    padding: 0;
}

.box{
    height: 300px;
    margin: 0 auto;
    background-color: red;
}
/*媒体查询*/
@media screen and (max-width: 768px){
    .box{
        width: 100%;
    }
}
@media screen  and (min-width: 768px) and (max-width: 992px){
    .box{
        width: 750px;
    }
}

@media screen  and (min-width: 992px) and (max-width: 1200px){
    .box{
        width: 970px;
    }
}

@media screen  and (min-width: 1200px){
    .box{
        width: 1170px;
    }
}
</style>
```

## 响应式布局hidden实现

媒体查询

```css
@media screen and (max-width: 768px){
    .hidden-xs{
        display: none;
    }
}
@media screen and (min-width: 768px) and (max-width: 992px){
    .hidden-sm{
        display: none;
    }
}

@media screen and (min-width: 992px) and (max-width: 1200px){
    .hidden-md{
        display: none;
    }
}

@media screen and (min-width: 1200px){
    .hidden-lg{
        display: none;
    }
}
```

# 