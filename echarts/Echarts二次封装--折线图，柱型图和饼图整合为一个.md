>普通项目中常用的可视化图表，可能也就折线图，柱型图和饼图，如果每次都需要重新配置一遍图表参数，也会很心累，以下是我将三表合一的开发实践。

##### 完成需求
  - 折线图，柱型图和饼图三图合一；
  - 图表数据与样式分离，只需要专注于```ResponseData```参数即可；
  - 可配置默认参数项，根据自身项目需求修改样式；
  - 完美搭配后端渲染，亦可前端操作；
  - 迁移原生无痛手术，只需实现```$.extend(obj,addObj)```

####功能
  - 可生成折线图，柱形图，饼图；
  - 图表可根据窗口变化自适应;
  - 可在构造函数原型链上加添加方法，赋予实例诸如：```show,hide```等功能；
####以后的Flag
想再用函数编程重写一遍，但最近都没时间，只有一个思路，先立一个Flag，不折腾不舒服斯基。
####代码结构
  ```
####
var one = document.getElementById("one");
####
var oneResponseData = {
         dom:one,
    	//控制标题。
    	title:'收入情况分析',
    	//控制显示多少根线，多少个柱子
    	legendData:['旅行社','酒店','景(区)点'],
        //控制线和柱子的高度。
        seriesData:[[30, 332, 201, 434, 190,],[434, 190,30, 332, 201],[134, 120,30, 132, 401]],
        //X轴日期数据
    	xData :['10月10日', '10月12日', '10月13日', '10月14日', '10月15日'],
    }
####
var oneMsg = {
       
        //设置线条或柱子颜色
        color:['#048f3C','#00BFFF','#1B1E7C','#E10074','#E10013','#FF6A6A','rgba(255,66,93,1)','#00FA9A','#006400','#B23AEE'],
        radius: null,
        roseType :null,
        legend:true,
        
        labelPosition:null,
        formatter: null,
        center: null,
        xname:'单位(人)',
        type:'line',
    }
####构造函数
var echartInstance
 echartInstance.prototype.redom
 echartInstance.init =function(msg){
    new echartInstance(msg);
}
####传参，实例化图表；
oneEcharts = echartInstance.init(msg,oneResponse)
```

####案例展示
![](http://upload-images.jianshu.io/upload_images/6095375-dc8ac58abd1d7675.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####以下贴出全部代码
```
    var one = document.getElementById("one");
    var two = document.getElementById("two");
    var three = document.getElementById("three");
    var four = document.getElementById("four");
    //one ,two ,three ,four：oneResponseData，twoResponseData threeResponseData，fourResponseData对应相应的数据对象，只需要修改title,legendData,seriersData,xData
    //其他的不用关，
    var oneResponseData = {
         dom:one,
    	//控制标题。
    	title:'收入情况分析',
    	//控制显示多少根线，多少个柱子
    	legendData:['旅行社','酒店','景(区)点'],
        //控制线和柱子的高度。
        seriesData:[[30, 332, 201, 434, 190,],[434, 190,30, 332, 201],[134, 120,30, 132, 401]],
        //X轴日期数据
    	xData :['10月10日', '10月12日', '10月13日', '10月14日', '10月15日'],
    }
    var twoResponseData = {
          dom:two,
    	title:'旅游收入环比',
    	legendData:[],
    	xData :['2016/5', '2017/6', '207/8',],
        seriesData:[[170, 232, 401,]],
    }
    var threeResponseData = {
        dom:three,
    	title:'旅游收入同比',
    	legendData:['旅行社','酒店'],
    	xData :['2016/5', '2017/6', '207/8',],
        seriesData:[[170, 232, 401,]],
    }
    var fourResponseData ={
         dom:four,
    	 title:'旅游收入占比',
         legendData:['旅行社','酒店'],
         seriesData:[
            {value:310, name:'旅行社'},
            {value:335, name:'酒店'},
            {value:235, name:'景点'},
        ],
        xData :'',
    }
```
```
//Echart图表的默认参数
     var oneMsg = {
       
        //设置线条或柱子颜色
        color:['#048f3C','#00BFFF','#1B1E7C','#E10074','#E10013','#FF6A6A','rgba(255,66,93,1)','#00FA9A','#006400','#B23AEE'],
        radius: null,
        roseType :null,
        legend:true,
        
        labelPosition:null,
        formatter: null,
        center: null,
        xname:'单位(人)',
        type:'line',
    }
    var twoMsg = {
        color:['#029060','#007BCB','#56187C','#E1005E','#E85604','rgba(230,179,64,1)','#FF7F24','rgba(39,73,98,1)',,'#8B1C62','#B03060'],
        radius: null,
        roseType :null,
        legend:true,
        labelPosition:null,
        formatter: null,
        center: null,

        xname:'单位(人)',
        type:'bar',
    }
    var threeMsg = {
    	
        
        color:['#CDB5CD','rgba(255,94,72,1)','#4EEE94','#B23AEE','rgba(173,137,118,1)','rgba(255,156,128,1)','rgba(0,34,40,1)','#FF7F24'],
        radius: null,
        roseType :null,
        legend:true,
        labelPosition:null,
        formatter: null,
        center: null,

        xname:'单位(人)',
        type:'bar',
    }
    var fourMsg = {
        color:['#3DB393','#FF727B','#438AFF','#00F5FF','#458B00','#9370DB','#7A67EE'],
        radius: [0, 80],
        roseType :false,
        legend:true,
        areaStyle:'',
        labelPosition:'inner',
        formatter: function(params){
            return Math.round(params.percent) === 0 ? '' : Math.round(params.percent)+"%"  ;
        },
        center: ['40%', '60%'],
        xname:'',
        type:'pie',
        
        
    }
```

```
###echarts构造函数
var echartInstance = function(msg){
        this.dom = msg.dom;
        this.chartStyle= {
            title:{
                top:'8%',
                left:'6%',
                text: msg.title,
                textStyle:{
                    fontSize:14,
                }
            },
            color:msg.color,

            grid:{
                left:'6%',
                top:'32%',
            },
            legend:{
                show:msg.legend,
                orient: msg.type === 'pie' ?'vertical':'horizontal',
                top:msg.type === 'pie' ?'48%' : '20%',
                left:msg.type === 'pie' ?'60%':'6%',
                width:'20%',
                height:'35%',
                itemGap:10,
                itemWidth:15,
                itemHeight:10,
                textStyle:{
                    color:'black',
                },
                data:msg.legendData,
            },
            xAxis: {
                type: 'category',
                splitLine: {
                    show: false
                },
                axisTick:{
                    show:false,
                },
                axisLine:{
                    show:false,
                },
                axisLabel:{
                	margin:20,
                	fontSize:22,
                },
                data: msg.xData,
            },
            yAxis: {
                type: 'value',
                // name:msg.xname,
                nameLocation:'end',
                nameTextStyle:{
                    align:'left'
                },
                boundaryGap: [0, '100%'],
                splitLine: {
                    show: false,

                },
                axisLine:{
                    show:false,
                },
                axisLabel:{
                    inside:true,
                    formatter:function(params){
                    	return params+'人'
                    }
                },
                axisTick:{
                    show:false,
                },
                splitLine:{
                    show:true,
                    lineStyle:{
                        color:'white',
                        shadowOffsetY:10,
                        shadowColor:'#DCDCDC',
                    }
                },
            },
            series:[
                ]
        };
        var _self = this;
        if (Object.prototype.toString.call(msg.seriesData[0]).slice(8,-1) === "Array") {
        	msg.seriesData.forEach(function(item,index){
        		var itemObj = {
        	            name: msg.legendData[index],
        	            type: msg.type,
        	            radius: msg.radius,
        	            center: msg.center,
        	            roseType: msg.roseType,
        	            label:{
        	                normal:{
        	                    show: false,
        	                    position: msg.labelPosition,
        	                    formatter: msg.formatter,
        	                },
        	                emphasis:{
        	                    show: false
        	                }
        	            },
        	            itemStyle:{
        	                normal:{
        	                    color:msg.color[index],
        	                    borderColor:msg.color[index],
        	                    borderWidth:20,
        	                },
        	            },
        	            lineStyle:{
        	                normal:{
        	                    color:msg.color[index]
        	                },
        	            },
        	            barWidth:10,
        	            barGap:'250%',
        	            smooth:true,
        	            showSymbol: false,
        	            hoverAnimation: false,
        	            data:item,
        	        }
        		_self.chartStyle.series.push(itemObj)
        	})
        }else{
        	_self.chartStyle.series = [{
                    name: '往年',
                    type: msg.type,
                    radius: msg.radius,
                    center: msg.center,
                    roseType: msg.roseType,
                    label:{
                        normal:{
                            show: true,
                            position: msg.labelPosition,
                            formatter: msg.formatter,
                        },
                        emphasis:{
                            show: false
                        }
                    },
                    itemStyle:{
                        normal:{
                            color:msg.areaStyle[0],
                            borderColor:'rgba(0,0,0,0)',
                            borderWidth:20,
                        },
                    },
                    barWidth:24,
                    smooth:true,
                    showSymbol: false,
                    hoverAnimation: false,
                    data:msg.seriesData,
                    lineStyle:{
                        normal:{
                            color:msg.color[0]
                        },
                    },
                },]
        }
    };

    echartInstance.prototype.redom=function(){
        var _self = this;
        _self.echart = echarts.init(_self.dom);
        var option = {
            title: this.chartStyle.title,
            color: this.chartStyle.color,
            legend: this.chartStyle.legend,
            tooltip: {
                trigger: 'axis',
                axisPointer: {
                    animation: false
                }
            },
            grid:this.chartStyle.grid,
            xAxis: this.chartStyle.xAxis,
            yAxis: this.chartStyle.yAxis,
            series: this.chartStyle.series,
        };          
        if (option && typeof option === "object") {
            _self.echart.setOption(option, true);
        }
        window.onresize = function(){
            setTimeout(_self.echart.resize(),100);
        }
    }
    echartInstance.init = function(msg,responseData){
    	$.extend(msg, responseData)
        var instance = new echartInstance(msg);
        instance.redom()
        return instance
    }
```

```
###初始化实例
    var oneEchart = echartInstance.init(oneMsg,oneResponseData);
    var twoEchart = echartInstance.init(twoMsg,twoResponseData);
    var threeEchart = echartInstance.init(threeMsg,threeResponseData);
    var fourEchart = echartInstance.init(fourMsg,fourResponseData);
```
