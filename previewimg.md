##按钮上传预览图片

###演示地址
[http://121.41.117.10:8888/share_doc/previewimg.html](http://121.41.117.10:8888/share_doc/previewimg.html)

###代码
	<!DOCTYPE html>
	<html lang="zh-CN" ng-app="app">
	<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
	<title>Bootstrap 101 Template</title>
	<link href="../css/bootstrap.min.css" rel="stylesheet">
	</head>
	<body>
	    <div>
	        <input type="file" id="uploadfile"  style="display:none;">
	       <button class="btn btn-success" onclick="uploadfile.click()">上传图片</button>
	   </div>
	   <div class="preview_img" style="display:none;">
	       <img src="#" />
	   </div>
	
	</body>
	 <script src="http://cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
	 <script>
	 $('#uploadfile').change(function(event) {
	     // 根据这个 <input> 获取文件的 HTML5 js 对象
	     var files = event.target.files, file;        
	     if (files && files.length > 0) {
	       // 获取目前上传的文件
	       file = files[0];
	       // 那么我们可以做一下诸如文件大小校验的动作
	       if(file.size > 1024 * 1024 * 5) {
	         alert('图片大小不能超过 5MB!');
	         return false;
	       }
	       // 获取 window 的 URL 工具
	       var URL = window.URL || window.webkitURL;
	       // 通过 file 生成目标 url
	       var imgURL = URL.createObjectURL(file);
	       $('.preview_img').find('img').attr('src',imgURL);
	       $('.preview_img').css({display:"block"});
	     }
	   });
	 </script>
	 </html>