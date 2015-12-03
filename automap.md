##百度地图自定定位当前位置

###演示地址
[http://121.41.117.10:8888/share_doc/automap.html](http://121.41.117.10:8888/share_doc/automap.html "自动定位")

###js代码
	var app = angular.module('app',[]);
	app.factory('Geo',['$q',function($q){
		return {
			currentLocation:function(){
				var deferred = $q.defer();
				//定位对象
				var geoc = new BMap.Geocoder();
				var geolocation = new BMap.Geolocation();
				geolocation.getCurrentPosition(function(r){
				  if(this.getStatus() == BMAP_STATUS_SUCCESS){
				    geoc.getLocation(r.point, function(rs){
				    	deferred.resolve(rs);
				    });
				  }
				},{enableHighAccuracy: true});
				return deferred.promise; 
			},
		}
	}]);
	
	app.controller('mapController',['$scope','Geo',
	function($scope,Geo){
		Geo.currentLocation().then(function(address){
			province = address.addressComponents.province;
			city = address.addressComponents.city;
			area = address.addressComponents.district;
			street = address.addressComponents.street;
			console.log(address.addressComponents);
			$scope.region = province+city+area+street;
		});
	}])


###html代码

	<!DOCTYPE html>
		<html lang="zh-CN" ng-app="app">
	 	<head>
	    <meta charset="utf-8">
	    <meta http-equiv="X-UA-Compatible" content="IE=edge">
	    <meta name="viewport" content="width=device-width, initial-scale=1">
	    <title>Bootstrap 101 Template</title>
	    <link href="../css/bootstrap.min.css" rel="stylesheet">
	  	 </head>
	   	<body ng-controller="mapController">
		<div class="top15">
	       <div class="col-xs-3 top15 text-right" >街道:</div>
	        <div class="col-xs-8 top15"><input ng-model="region" class="form-control input-sm" placeholder="正在定位..." type="text">
	        </div>
	        <div style="margin-left: -30px;margin-right: -15px;" class="col-xs-1 top15 " ng-click='currentLoaction()'><span  class="glyphicon glyphicon-map-marker" aria-hidden="true"></span></div>
	    </div>
	    <script src="http://cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
		<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=WLOqkkobfdvnA6BYB0CMhc4k"></script>
		<script type="text/javascript" src="../js/angular.min.js"></script>
		<script type="text/javascript" src="../js/share.js"></script>
	 	 </body>
		</html>