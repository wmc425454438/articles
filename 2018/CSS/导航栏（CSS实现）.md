# CSS导航栏

关键点是` ul `和` li `的隐藏和显示，隐藏用` display: none `，显示用` display: block `。

还有其中的一些布局和动画效果，当然` webkit `核心的浏览器才能看到动画效果，也可以添加其他核心的浏览器动画效果。
``` html
<!DOCTYPE html>
<html>
<head>
	<title>css导航栏</title>
	<style type="text/css">
		.admin_sidebar_list{
			list-style-type: none;
		}
		.admin_sidebar_list>li input{
			position: absolute;
			width: 100%;
			height: 48px;
			z-index: 10;
			opacity: 0;
		}
		.admin_sidebar_list>li ul{
			margin: 0 0 0 0;
			list-style-type: none;
			padding-left: 20px;
			display: none;
		}
		.admin_sidebar_list>li input:checked ~ul{
			display: block;
			-webkit-animation-duration: 1s;
			-webkit-animation-name: opacityChange;
		}

		@-webkit-keyframes opacityChange {
			0%{
				opacity: 0.3;
			}
			50%{
				opacity: 0.8;
			}
			100%{
				opacity: 1;
			}
		}
	</style>
</head>
<body>
<ul class="admin_sidebar_list">
	<li><input type="checkbox" name="nav1" checked="checked">
	<a>目录1</a>
	<ul>
		<li><a>子目录1</a></li>
		<li><a>子目录2</a></li>
	</ul>
	</li>
	<li><input type="checkbox" name="nav2">
	<a>目录2</a>
	<ul>
		<li><a>子目录3</a></li>
		<li><a>子目录4</a></li>
	</ul>
	</li>
</ul>
</body>
</html>
```
