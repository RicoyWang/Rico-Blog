>看了很多文章对Vue中如何使用写的都很乱，下面贴一下自己做了简单的方法提取，还有开发过程中遇到的神坑。
######注意事项
- 首先要在index.html 的head标签中要挂载```<script type="text/javascript" src="http://api.map.baidu.com/getscript?v=2.0&ak=str&t=20170517145936"></script>```，```str```为申请获得的一段字符串，```v```为版本也需要添加上。
- 百度地图中如果需要添加**实时交通**，这类悬浮框的控件，需要另外引入CSS、JS文件，如：```TrafficControl_min.js```，```TrafficControl_min.css```，热地图需引入```heatmap.js```
- 百度地图热力图，需要大量坐标才能生成不同色块，methods方法中的addHot是我自己写的随机生成坐标点函数，根据地图中心点来计算。
###正文
Vue实例上的```methods```存储各个空间的方法，在```mounted```中进行初始化渲染。
######功能目录
- ```addLineVideo``` --地图中心处生成牵引线，线条起始及终点位置固定，自己写的比较粗糙。
- ```mapMoveSelf```--地图缩放及视觉中心点地理位置重置。
- ```addIcon```-- 地图上的图标。
- ```addMenu```-- 添加放大缩小控件
- ```addLocaPosition```-- 添加定位控件
- ```addControl``` -- 添加工具条，比例尺控件
- ```addControlCityList``` -- 添加城市下拉列表控件
- ```addLine``` -- 添加曲线
- ```addCustControl``` -- 自定义放大缩小悬浮控件
- ```addLoad``` -- 实时路况 **需要引入额外文件**
- ```addHot``` -- 热力图   **需要引入额外文件**
```
methods部分挂在各功能函数
mapMoveSelf(map){
    console.log(map)
    setTimeout(function(){
        map.panTo(new BMap.Point(119.906441,29.457793));
    }, 1000);
    setTimeout(function(){
        map.setZoom(14);
    },4000);
},
addIcon(map){
    var points = [
            [120.018874,29.48683],
            [119.923671,29.514494],
            [119.906441,29.457793],
            [119.965029,29.403953],
            [119.915029,29.303953],
            [119.935029,29.203953],
            [119.945029,29.453953],
            [119.765029,29.363953]
        ];
    // 向地图添加标注
    for( var i = 0;i < points.length; i++){
    //定义新图标
    var myIcon = new BMap.Icon(require("../../../assets/images/wifi.png"), new BMap.Size(44, 44), {
    // 指定定位位置
    offset: new BMap.Size(10, 25),
    // 当需要从一幅较大的图片中截取某部分作为标注图标时，需要指定大图的偏移位置 
    //imageOffset: new BMap.Size(0, 0 - i * 25)  设置图片偏移 
    });
    var point = new BMap.Point(points[i][0],points[i][1]);
    // 创建标注对象并添加到地图 
    var marker = new BMap.Marker(point,{icon: myIcon});
    map.addOverlay(marker);
    };
    //添加新图标的监听事件
    marker.addEventListener('click',function(){
        var p = marker1.getPosition();//获取位置
        // alert("点击的位置是：" + p.lng + ',' + p.lat);
    })
},
addMenu(map){
    var menu = new BMap.ContextMenu();

    var txtMenuItem = [
        {
            text:'放大',
            callback:function(){map.zoomIn()}
        },
        {
            text:'缩小',
            callback:function(){map.zoomOut()}
        }
    ];
    for(var i=0; i < txtMenuItem.length; i++){
        menu.addItem(new BMap.MenuItem(txtMenuItem[i].text,txtMenuItem[i].callback,100));
    }
    map.addContextMenu(menu);
},
addLocaPosition(map){
    //添加定位组件
    var geolocationControl = new BMap.GeolocationControl();
    geolocationControl.addEventListener("locationSuccess", function(e){
    // 定位成功事件
    var address = '';
    address += e.addressComponent.province;
    address += e.addressComponent.city;
    address += e.addressComponent.district;
    address += e.addressComponent.street;
    address += e.addressComponent.streetNumber;
    alert("当前定位地址为：" + address);
    });
    geolocationControl.addEventListener("locationError",function(e){
    // 定位失败事件
    alert(e.message);
    });
    map.addControl(geolocationControl)
},
addControl(map){
    //添加工具条、比例尺控件
    map.addControl(new BMap.ScaleControl({anchor:BMAP_ANCHOR_TOP_LEFT}));
    map.addControl(new BMap.NavigationControl());
    map.addControl(new BMap.NavigationControl({anchor:BMAP_ANCHOR_BOTTOM_RIGHT,type:BMAP_NAVIGATION_CONTROL_SMALL}));
    map.addControl(new BMap.NavigationControl({anchor: BMAP_ANCHOR_TOP_LEFT,type: BMAP_NAVIGATION_CONTROL_LARGE,enableGeolocation: true}));
},
addControlCityList(map){
    //创建城市下拉列表控件；
    map.addControl(new BMap.CityListControl({
        anchor:BMAP_ANCHOR_BOTTOM_RIGHT,
        offset:new BMap.Size(130,23),
        //切换城市之间事件
        onChangeBefore: function(){
           alert('before');
        },
        //切换城市之后事件
        onChangeAfter:function(){
          alert('after');
        }
    }));
},
addLine(map){
    //添加曲线
    var beijingPosition=new BMap.Point(116.432045,39.910683),
    hangzhouPosition=new BMap.Point(120.129721,30.314429),
    taiwanPosition=new BMap.Point(121.491121,25.127053);
    var point = [beijingPosition,hangzhouPosition,taiwanPosition];

    var curve = new BMapLib.CurveLine(point, {strokeColor:"blue", strokeWeight:3, strokeOpacity:0.5});//创建弧线
    map.addOverlay(curve);//添加到地图上
    curve.enableEditing();//开启编辑功能
},
addCustControl(map){
    //创建自定义放大缩小悬浮控件
    function ZoomControl() {
    //默认停靠位置和偏移量
    this.defaultAnchor = BMAP_ANCHOR_BOTTOM_RIGHT;
    this.defaultOffset = new BMap.Size(50,23);
    }
    //通过JavaScript的prototype属性继承于BMap.Control
    ZoomControl.prototype = new BMap.Control();
    //自定义控件必须实现自己的initialize方法，并且将控件的DOM元素返回
    //在本方法中创建div容器，并将其添加到地图容器中
    ZoomControl.prototype.initialize = function(map) {
        //创建一个DOM元素
        var div = document.createElement('div');
        //添加文字说明
        div.appendChild(document.createTextNode('放大两级'));
        //添加样式
        div.style.color = '#40C5CC'; 
        div.style.cursor = 'pointer';
        div.style.border = '3px solid gray';
        div.style.backgroundColor = '#eee';
        //绑定事件，点击触发
        div.onclick = function(e) {
            map.setZoom(map.getZoom() + 2);
        }
        //添加DOM元素到地图上
        map.getContainer().appendChild(div);
        //将DOM元素返回
        return div;
    }
    //创建控件
    var myZoomCtrl = new ZoomControl();
    map.addControl(myZoomCtrl)
},
addLoad(map){
    //实时路况
    var ctrl = new BMapLib.TrafficControl({
        showPanel: true //是否显示路况提示面板
    });      
    map.addControl(ctrl);
    ctrl.setAnchor(BMAP_ANCHOR_BOTTOM_RIGHT);  
},
```


```
mounted方法中初始化各功能部件
// 百度地图API功能
    // 创建Map实例
    const _self= this;
    var map = new BMap.Map("XSDFXPage",{enableMapClick:true});
    // 初始化地图,设置中心点坐标和地图级别
    map.centerAndZoom(new BMap.Point(120.018874,29.48683), 9);
    // 添加地图类型控件
    // map.addControl(new BMap.MapTypeControl());  
    // 设置地图显示的城市 此项是必须设置的
    map.setCurrentCity("浦江");    
    // 开启鼠标滚轮缩放      
    map.enableScrollWheelZoom(true);
    // 设置定时器，对地图进行自动移动
    // this.mapMoveSelf
    /************************************************
    添加折线
    *************************************************/
    var pointGZ = new BMap.Point(119.923671,29.514494);
    var pointHK = new BMap.Point(110.35,20.02);
    setTimeout(function(){
        var polyline = new BMap.Polyline(
              [pointGZ,pointHK],
              {strokeColor:"blue",strokeWeight:5,strokeOpacity:0.5}
          );
         map.addOverlay(polyline);
     },6000);
    _self.addLoad(map);
     _self.addControl(map);
    _self.addLocaPosition(map);
    /************************************************
    添加自定义控件类，放大ZoomControl
    *************************************************/

    /************************************************
    添加添加城市列表控件
    *************************************************/
     _self.addControlCityList(map);
    /************************************************
    添加新图标
    *************************************************/
     _self.addIcon(map);
    /************************************************
    添加曲线
    *************************************************/

    /************************************************
    给地图添加右键菜单
    *************************************************/
    _self.addMenu(map);
```
实用工具网站
[百度地图·JavaScript](http://lbsyun.baidu.com/index.php?title=jspopular)
[百度地图坐标拾取](http://api.map.baidu.com/lbsapi/getpoint/index.html)
[百度地图API](http://developer.baidu.com/map/reference/index.php?title=Class:%E8%A6%86%E7%9B%96%E7%89%A9%E7%B1%BB/Polyline)
