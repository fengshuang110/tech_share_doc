## angular js 实现三级或者N级联动下拉菜单##

###演示地址
[http://121.41.117.10:8888/share_doc/category.html](http://121.41.117.10:8888/share_doc/category.html "演示地址")

需要在html和js代码中添加bootstrap样式和angularjs的引入路径
###html代码
		<!DOCTYPE html>
		<html lang="zh-CN" ng-app="app">
	 	<head>
	    <meta charset="utf-8">
	    <meta http-equiv="X-UA-Compatible" content="IE=edge">
	    <meta name="viewport" content="width=device-width, initial-scale=1">
	    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
	    <title>Bootstrap 101 Template</title>
	
	    <!-- Bootstrap -->
	    <link href="../css/bootstrap.min.css" rel="stylesheet">
	
	    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
	    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
	    <!--[if lt IE 9]>
	      <script src="//cdn.bootcss.com/html5shiv/3.7.2/html5shiv.min.js"></script>
	      <script src="//cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>
	    <![endif]-->
	  	 </head>
	   	<body ng-controller="categoryController">
			<input id="region_id" type="hidden">
	          <div ng-class="{'col-sm-4':num==3,'col-sm-6':num==2,'col-sm-12':num==1}"  class="clear-left" >      
				<select  class=" form-control index-main" ng-change="children_cate()"  ng-options="item.name for item in categories"  ng-model="category">
					<option value="">选择分类</option>
				</select>
			  </div>
			  <div ng-class="{'col-sm-4':num==3,'col-sm-6':num==2,'col-sm-12':num==1}">  
				<select  class="form-control index-main" ng-change="children_cate1(category1)"  ng-show="two_cates.length>0"  ng-model="category1" >
					<option ng-repeat="item in two_cates" value="{{item.id}}">{{item.name}}</option>
				</select>
			  </div>
			  <div ng-class="{'col-sm-4':num==3,'col-sm-6':num==2,'col-sm-12':num==1}" class="clear-right">  
				<select  class="form-control index-main" ng-change="children_cate2(category2)" ng-show="third_cates.length>0" ng-model="category2" >
					<option ng-repeat="item in third_cates" value="{{item.id}}">{{item.name}}</option>
				</select>
			  </div> 
			
	    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
	    <script src="http://cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
	    <!-- Include all compiled plugins (below), or include individual files as needed -->
	    <script src="../js/bootstrap.min.js"></script>
		
		<script type="text/javascript" src="../js/angular.min.js"></script>
		<script type="text/javascript" src="../js/share.js"></script>
	
	 	 </body>
		</html>


###js代码 
初始化以及下拉菜单
二级下拉菜单通过ajax从后端获取，传递的参数我pid

		var app = angular.module('app',[]);
	
	var domain = "http://121.41.117.10:8889/"; 
	app.factory('Citycategory',function() {
		return {
			region: function(data) {
				return $.get(domain+'city/region',data);
			},
		}
	});
	app.controller('categoryController', ['$scope','Citycategory',
	function($scope,Citycategory){
	$scope.two_cates = [];
	$scope.third_cates = [];
	Citycategory.region().success(function(res){
		$scope.categories = res;
		$scope.$apply();
	});
	$scope.num = 1;
	$scope.children_cate = function(){
		$scope.category_id = $scope.category.id;
		$("#region_id").val($scope.category.id);
		Citycategory.region({id:$scope.category_id}).success(function(res){
			$scope.two_cates = res;
			$scope.third_cates=[];
			if(res.length>0){
				$scope.num = 2;
			}else{
				$scope.num = 1;
			}
			$scope.$apply();
		});
	}
	$scope.children_cate1 = function(category1){
		$("#region_id").val(category1);
		Citycategory.region({id:category1}).success(function(res){
			$scope.third_cates = res;
			if(res.length>0){
				$scope.num = 3;
			}else{
				$scope.num = 2;
			}
			$scope.$apply();
		});
	}
	$scope.children_cate2 = function(category2){
		$("#region_id").val(category2);
	}
	}]);


