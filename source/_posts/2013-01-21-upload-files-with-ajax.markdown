---
layout: post
title: "用Ajax方式上传文件"
date: 2013-01-21 19:03
comments: true
categories: 
---

在HTML5出现之前，无刷新文件上传的思路一般是使用frame或flash。有一天，HTML5
带来了XMLHttpRequest第二版，它为Ajax上传文件带来的可能。所以，如果还要支持
IE6什么的就不必考虑这个方法了，甚至IE9也不行。
<!--more-->

![Alt Ajax上传图片](http://bcs.duapp.com/picfile/2013/1/b87d6d2069d4e7f1.png)

实现Ajax上传文件要用到两个对象：FormData, XMLHttpRequest。下面介绍这两个对象。

###FormData

HTML5增加了一个FormData对象，FormData对象的作用是模拟表单数据，然后就可以用
Ajax的方式提交这个表单的数据，跟用正常方式提交表单一样。是不是很方便。

使用FormData的方式：
	var formData = new FormData();			//new一个表单对象
	formData.append('field1', 'value1');	//添加一个name为'field1',值为'value1'的表单项表单项
	formData.append('id', '123456');		//添加一个name为'id'的表单项

我们可以使用Ajax上传文件的一个基础就是，FormData可以包含文件表单项。如我们有选择文件的表单：

	<input type="file" id='selfile'>

我们可以这样把它添加进FormData对象。

	var file = document.getElementById("selfile").files[0];
	formData.append('picfile', file);

FormData了解到这里已经足够了，下面就要的就是把'formData'发送到服务器。这就要用到*XMLHttpRequest*对象了。


###XMLHttpRequest对象

XMLHttpRequest对象是Ajax的基础，用于在浏览器与服务器交换数据。

XMLHttpRequest的用法：

	var xhr = new XMLHttpRequest();	//新建一个XMLHttpRequest对象
	xhr.open('POST', '/jsUpload');	//表单提交的地址
	xhr.onload = function(){		//当服务器返回时，会调用函数
		if(xhr.status == 200){
			console.log('上传成功');
		}
		else{
			console.log('好像出错了，你查查呗'）；
		}
	}
	//假定已经有formData对象
	xhr.onload.send(formData);		//发送数据

###进度条支持
XMLHttpRequest第二版还增加了一个`onprogress`回调接口。有了这个实现进度条就很简单了。
	
###完整的示例

HTML文件添加选择文件的表单。

	<input type="file" id='selfile'>

Javascript监听表单的改变事件，并在这个时间上传文件。

	var selFile = $('#selFile');
	selFile.change(function(e){
		if(window.FormData){		//判断浏览器是否支持FormData对象
			var formData = new FormData();
			var file = e.target.files[0];
			formData.append('picfile', file);

			var xhr = new XMLHttpRequest();
			xhr.open('POST', '/jsUpload');

			xhr.onload = function(){
				alert('上传完成');

				if(xhr.status < 200 || xhr.status > 299){
					alert('好像出了点问题');
				}
			}

			xhr.upload.onprogress = function (e){
				console.log('上传了....' + e.loaded / e.total * 100 + '%');			//进度信息
			}
			
			xhr.send(formData);
		}
	});


