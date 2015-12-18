# Tizen-App-MathTool

## 概述

一个简单的数学工具，很方便的把用户输入的一串数字转化为柱状图，折线图或者饼状图，再通过截屏等手段导出图片，使得数据大小关系一目了然。
## 算法介绍

Mathtool基于TIZEN web project开发，主要使用了Html与Javascript技术。通过save存储用户输入的数字，zhuChart、zheChart和bingChart方法中绘出对应的图像。
```js
localStorage.clear;  //清空内存

var number=[]; //保存数据到数组

function save(){
	var num=document.getElementById("number").value;
	number.push(num);
	};
			
function zhuChart(){
	var canvas=document.getElementById("myCanvas");  //获得绘图环境
	var context=canvas.getContext("2d");
	var width_canvas=parseInt(canvas.width);//设置画布参数
	var height_canvas=parseInt(canvas.height);
	context.translate(width_canvas/10,height_canvas*4/5);//调节绘图基准
				context.clearRect(-width_canvas/10,-8*height_canvas/10,width_canvas,height_canvas);
				draw_Sites(context,height_canvas,width_canvas);
    var max=0;                          //数据处理
	for(var i=0;i<number.length;i++){
		if(max<number[i])  
		 max=number[i];
		}
    var per=width_canvas*0.8/(2*number.length+1);
	var pos_num=0;
	if(number.length<5){
		pos_num=per/2;
		}else if(number.length>=5 && number.length<10){
		pos_num=per/4;
		}else{
			pos_num=0;
		}
		for(var i=0;i<number.length;i++){
		    var x=(2*(i+1)-1)*per;
			var y=-number[i]/max*(0.65*height_canvas);
			context.beginPath();
			context.moveTo(x,0);
			context.lineTo(x,y);
			context.lineTo(x+per,y);
			context.lineTo(x+per,0);
			context.closePath();
			context.fillStyle="black";
			context.fill();
			context.strokeText(number[i],x+pos_num,y-5);
		}
		context.translate(-width_canvas/10,-height_canvas*4/5);//调节绘图基准
	}
				
	function zheChart(){
		var canvas=document.getElementById("myCanvas");  //获得绘图环境
		var context=canvas.getContext("2d");
		var width_canvas=parseInt(canvas.width);//设置画布参数
		var height_canvas=parseInt(canvas.height);
		context.translate(width_canvas/10,height_canvas*4/5);//调节绘图基准
				context.clearRect(-width_canvas/10,-8*height_canvas/10,width_canvas,height_canvas);
				draw_Sites(context,height_canvas,width_canvas);

         var max=min=0;                          //数据处理
				for(var i=0;i<number.length;i++){
				    if(max<number[i])  
					    max=number[i];
					if(min>number[i])
					    min=number[i];
				}
                var per=width_canvas*0.8/(number.length+1);
				var pos_num=0;
				if(number.length<5){
					pos_num=per/2;
				}else if(number.length>=5 && number.length<10){
					pos_num=per/4;
				}else{
					pos_num=0;
				}

				context.beginPath();
                var x0=per;
				var y0=-number[0]/max*(0.65*height_canvas);
				context.moveTo(x0,y0);
				for(var i=0;i<number.length;i++){
				    var x=((i+1))*per;
					var y=-number[i]/max*(0.65*height_canvas);
					context.lineTo(x,y);
				}
				context.strokeStyle="black";
				context.stroke();

				for(var i=0;i<number.length;i++){
				    var x=((i+1))*per;
					var y=-number[i]/max*(0.65*height_canvas);
					context.beginPath();
			        context.arc(x,y,5,0,Math.PI*2,false); //绘制圆弧
			        context.closePath();
			        context.fillStyle="rgba(0,0,0,0.5)";
			        context.fill();
					context.strokeText(number[i],x-5,y-8);
			    }
				context.translate(-width_canvas/10,-height_canvas*4/5);//调节绘图基准
			};

			function bingChart(){
				var canvas=document.getElementById("myCanvas");  //获得绘图环境
			    var context=canvas.getContext("2d");
				var width_canvas=parseInt(canvas.width);          //设置画布参数
				var height_canvas=parseInt(canvas.height);
				context.translate(width_canvas/2,height_canvas/2);//调节绘图基准
				context.clearRect(-width_canvas/2,-height_canvas/2,width_canvas,height_canvas);
				
                var add=0;                          //数据处理
				for(var i=0;i<number.length;i++){
					add += parseFloat(number[i]); 
				}
                var cont_deg=0;
				var deg=0;
				for(var i=0;i<number.length;i++){
				    deg=number[i]/add*Math.PI*2;        //获得占比角度
                    cont_deg=cont_deg+parseFloat(deg);
					context.beginPath();
					context.moveTo(0,0);
					context.lineTo(width_canvas/3*Math.cos(cont_deg),width_canvas/3*Math.sin(cont_deg));
                    context.arc(0,0,width_canvas/3,cont_deg,cont_deg-deg,true);
			        context.closePath();
					var color=Math.round(Math.random()*1000000);
					context.fillStyle="#"+color;
					context.fill();
					context.strokeText(number[i],width_canvas*3/8*Math.cos(cont_deg-deg/2),width_canvas*3/8*Math.sin(cont_deg-deg/2));
				}
				context.translate(-width_canvas/2,-height_canvas/2);//调节绘图基准
			};
            
			function draw_Sites(context,height_canvas,width_canvas){  //绘制坐标轴
				var x1=8*width_canvas/10; //作图对角线
				var y1=-7*height_canvas/10;

			    context.beginPath(); //绘制坐标轴
			    context.moveTo(0,y1);
			    context.lineTo(0,0);
			    context.lineTo(x1,0);
			    context.strokeStyle="rgba(0,0,0,1)";
			    context.stroke();

				var m1=height_canvas/80;//绘制箭头一
				var n1=width_canvas/30;
				context.beginPath();
				context.moveTo(-m1,y1+n1);
				context.lineTo(0,y1);
				context.lineTo(m1,y1+n1);
				context.strokeStyle="rgba(0,0,0,1)";
			    context.stroke();

				var m2=width_canvas/30;//绘制箭头2
				var n2=height_canvas/80;
				context.beginPath();
				context.moveTo(x1-m2,n2);
				context.lineTo(x1,0);
				context.lineTo(x1-m2,-n2);
				context.strokeStyle="rgba(0,0,0,1)";
			    context.stroke();
			}
```
