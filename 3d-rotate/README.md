# 3D照片墙的无缝切换
最终效果

![pic.gif](pic.gif)

通过点击body可以切换照片墙

具体看一下html代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>3d-照片墙旋转</title>
	<link rel="stylesheet" type="text/css" href="index.css">
	<script src="jquery-3.2.1.js"></script>
</head>
<body>
	<div class="container">
		<ul class="container-3d">
			<li class="item"><img src="img/Iphone1.png" alt="" class="pic-item"></li>
			<li class="item"><img src="img/Iphone2.png" alt="" class="pic-item"></li>
			<li class="item"><img src="img/Iphone3.png" alt="" class="pic-item"></li>
			<li class="item"><img src="img/Iphone1.png" alt="" class="pic-item"></li>
			<li class="item"><img src="img/Iphone2.png" alt="" class="pic-item"></li>
			<li class="item"><img src="img/Iphone3.png" alt="" class="pic-item"></li>
		</ul>
	</div>
</body>
</html>
```

主要思路就是设置3D场景 => 旋转，我们把container看做是一个一个盒子，在这个盒子中放入一个六面体container-3d，然后让这个六面体旋转就行了。

1. 把class为container的元素设置为3D场景，并让该元素垂直居中。(这个就相当于盒子)

   ```css
   /* div垂直居中显示 */
   .container{
   	position: absolute;
   	transform-style: preserve-3d; /* 设置3d场景 */
   	transform: perspective(1500px);	/*设置景深*/
   	margin: 0 auto;
   	width: 384px;
   	height: 550px;
   	top: 0;
   	left: 0;
   	right: 0;
   	bottom: 0;
   }
   ```

   2.给container的子元素container-3d同样也设置为3D场景,(这个就相当于六面体)

   ```css
   .container-3d{
   	width: 100%;
   	height: 100%;
   	position: absolute;
   	transform: rotateY(0deg);
       transform-origin: center bottom 0; 
       transform-style: preserve-3d; 
       transition: all 1s ease-in-out;	/*设置以后旋转的的动画*/
   }
   ```

   3.给这个六面体添加每个面添加图片，通过js让每个面先分别沿着Y轴旋转0 60 120 180 240 300度，再相对于旋转完后的位置往Z轴偏移300px

   ```javascript
   		var rotate = 60; //旋转的角度
   		var moveY = 6; //每次旋转的角度
   		var moveZ = '300px'; //偏移的的距离
   		var liLists = $('.item');
   		var j = 1; 
   		var rotateArr = [] //存放每个元素旋转的度数
           /*动过js给六面体的每个面添加图片*/
   		for(var i = 0; i < liLists.length; i++){
   			$(liLists[i]).css({
   				"transform": "rotateY("+(i*rotate)+"deg) translateZ("+moveZ+")",
   			})
   			rotateArr.push(i*rotate) //记录初始化每个item的rotateY的值
   		}
   ```

   **注意**

   在这儿给给每个面添加图片的时候要注意设置rotateY和translateZ先后顺序性，如果是设置translateZ，在设置rotateY就会有意想不到的BUG

   4.点击旋转，这儿就很简单了，就是让这个六面围绕着Y轴旋转就行了，记住不是让六面旋转。而是让整个六面体旋转。

   ```javascript
         //点击改变一下位置
         $('body').click(function(){
             $('.container-3d').css({
                 "transform": "rotateY("+(j*rotate)+"deg)",
                 "-ms-transform": "rotateY("+(j*rotate)+"deg)"
             })
             j++
         })
   ```

   每点击一下，这个盒子就旋转60，在结合前面的动画就能够完成漂亮的旋转了

   ```css
    transition: all 1s ease-in-out;	
   ```

   ​