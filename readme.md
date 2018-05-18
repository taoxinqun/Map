- [人员位置图——治安](#%E4%BA%BA%E5%91%98%E4%BD%8D%E7%BD%AE%E5%9B%BE%E2%80%94%E2%80%94%E6%B2%BB%E5%AE%89)
    - [1. 简介](#1-%E7%AE%80%E4%BB%8B)
    - [2. 编撰](#2-%E7%BC%96%E6%92%B0)
    - [3. 演示](#3-%E6%BC%94%E7%A4%BA)
    - [4. PIXI.JS背景图](#4-pixijs%E8%83%8C%E6%99%AF%E5%9B%BE)
    - [5. 坐标转换](#5-%E5%9D%90%E6%A0%87%E8%BD%AC%E6%8D%A2)
        - [5.1 CAD图坐标转PIXI.JS背景图坐标](#51-cad%E5%9B%BE%E5%9D%90%E6%A0%87%E8%BD%ACpixijs%E8%83%8C%E6%99%AF%E5%9B%BE%E5%9D%90%E6%A0%87)
        - [5.2 PIXI.JS背景图逆时针旋转90°](#52-pixijs%E8%83%8C%E6%99%AF%E5%9B%BE%E9%80%86%E6%97%B6%E9%92%88%E6%97%8B%E8%BD%AC90%C2%B0)
    - [6. 文档](#6-%E6%96%87%E6%A1%A3)
    - [7. 使用](#7-%E4%BD%BF%E7%94%A8)
    - [8. 使用](#8-%E4%BD%BF%E7%94%A8)
    - [9. 补充](#9-%E8%A1%A5%E5%85%85)
    
# 人员位置图——治安

## 1. 简介

 编写该文档的目的：记录转换流程。

## 2. 编撰
 陶新群(在原来基础上重新整理，方便后期维护)
 
## 3. 演示

 * [治安-横版](https://forwardnow.github.io/LocationMap/dist/index.html)
 * [治安-竖版](https://forwardnow.github.io/LocationMap/dist/index_vertical_2.html)

## 4. PIXI.JS背景图

![PIXI.JS背景图制作](doc/images/1.jpg)

**PIXI.JS背景图尺寸 在PSD中可以量取**

 * 宽度：1920 px
 * 高度： 650 px
 
**装饰图 尺寸  酷家乐装修效果图的分辨率设置，分辨率越大，图片越清晰，图片质量越好，图片K数就越大（打开时速度会变慢）**

 * 宽度：3200 px
 * 高度：2400 px

 酷家乐里，保存图片时，图片的分辨率大小（即保存图片质量的清晰度）
 

## 5. 坐标转换

### 5.1 CAD图坐标转PIXI.JS背景图坐标

![CAD图坐标转PIXI.JS背景图坐标](doc/images/2.jpg)

    设
        1. CAD图：宽度 cad_w, 高度 cad_h, X轴偏移量 cad_offset_x, Y轴偏移量 cad_offset_y。
        2. 背景图：宽度 bg_w, 高度 bg_h, X轴偏移量 bg_offset_x, Y轴偏移量 bg_offset_y。
        3. CAD图上任意一点(x,y) 对应 背景图上的一点(x',y')
    则
        x' - bg_offset_x = ( x - cad_offset_x ) / ( cad_w / bg_w )
        - ( y' - bg_offset_y ) + bg_h  = ( y - cad_offset_y ) / ( cad_h / bg_h )
    即
        x' = ( x - cad_offset_x ) / ( cad_w / bg_w ) + bg_offset_x
        y' = - ( y - cad_offset_y ) / ( cad_h / bg_h ) + bg_offset_y + bg_h 
        
**CAD图有效区域参数**

  * 宽度  46341
  * 高度  13841
  * X轴偏移量   962
  * Y轴偏移量   714
  

**PIXI.JS背景图有效区域参数**

  * 宽度  1767
  * 高度  529
  * X轴偏移量   72
  * Y轴偏移量   77
  
**校验**
  
  * CAD图参数 有效区域 宽高比:     46341 / 13841 = 3.348096235821111
  * PIXI.JS背景图 有效区域 宽高比:  1767 /   529 = 3.340264650283554
  
**对应关系**

    x' = ( x - 962 ) / 26.225806451612904 + 72
    y' = ( y - 714 ) / -26.16446124763705 + 606

![CAD图坐标PSD背景图坐标通俗的量取方法](doc/images/0.jpg)   
tip: 在PSD量取背景图的X Y偏移时，要去掉墙体的厚度，最终得到的才是背景图的宽高度，也就是有效区域。

### 5.2 PIXI.JS背景图逆时针旋转90°

![PIXI.JS背景图逆时针旋转90°](doc/images/3.jpg)

    设
        画布尺寸为 w * h，
        旋转之前的任意一点(x,y)对应旋转之后的(x’,y’),
    则
        x’= y
        y’= -(x-w)


### 5.3 PIXI.JS背景图顺时针旋转90°

![PIXI.JS背景图顺时针旋转90°](doc/images/4.png)

    设
        画布尺寸为 w * h，
        旋转之前的任意一点(x,y)对应顺时针90°旋转之后的(x’,y’),
    则
        x’= y
        y’= x

    “横版”

    宽度：1920 px
    高度： 650 px

    在PSD中对画布进行顺时针90°旋转 再进行水平翻转。

    【画布】：图片

    旋转后的尺寸

    宽度： 650 px
    高度：1920 px
    坐标

    互换 x 、y 坐标



## 6. 文档

    LocationMap\
        doc\
            ground.psd
            ground_vertical.psd
            zhian-0719-2.dwg
            zhian-0719-2.dxf
            PIXI.JS背景图制作.psd
            坐标转换——CAD图坐标转PIXI.JS背景图坐标.psd
            坐标转换——PIXI.JS背景图逆时针旋转90°.psd
            
            images\
                1.jpg
                2.jpg
                3.jpg
  
  
  * `ground.psd` 根据CAD图制作的2D效果图，尺寸 1920*650。
  * `ground_vertical.psd` 对`ground.psd`逆时针旋转90°后得到的2D效果图。
  * `zhian-0719-2.dxf` 原始CAD图。不规范，无法导入酷家乐直接使用。
  * `zhian-0719-2.dwg` 对原始CAD图进行重画得到。
  
## 7. 使用及坐标转换说明
 
**全局变量**

`window.LocationMap`


  求 2D图(x,y) 与 CAD图(x',y') 的坐标映射关系 f1、f2

  x = f1(x')
  y = f2(y')
  即，求

  x = x' / ratio_width + delta_x
  y = y' / ratio_height + delta_y
  因此，需要求出

  ratio_width CAD图有效区域的宽度 与 2D图有效区域的宽度 的比值
  ratio_height CAD图有效区域的高度 与 2D图有效区域的高度 的比值
  delta_x X轴方向的差值
  delta_y Y轴方向的差值

  ###7.1 CAD源文件参数
    CAD图参数  ![CAD源文件](doc/images/CAD_args.jpg)
    CAD源文件读取坐标值
    宽度 46341
    高度 13841
    X轴偏移量 962
    Y轴偏移量 714

  ###7.1 2D图参数
    2D图参数  ![2D-JPG图参数,PSD中量取](doc/images/2D_args.jpg)

    宽度 1767
    高度 529
    X轴偏移量 72
    Y轴偏移量 77

  校验

  CAD图参数 宽高比: 46341 / 13841 = 3.348096235821111
  CAD图参数 宽高比: 1767 / 529 = 3.340264650283554


  ###7.2 求比例（ratio）
    ratio_width = 46341 / 1767 = 26.225806451612904

    ratio_height = 13841 / 529 = 26.16446124763705

  ###7.3 求差值（delta）
    2D图的坐标原点在左上角，CAD图的坐标原点在左下角

    x - 72 = ( x' - 962 ) / ratio_width

    529 - ( y - 77 ) = ( y' - 714 ) / ratio_height

  ###7.4 综上
    x = ( x' - 962 ) / 26.225806451612904 + 72

    y = ( y' - 714 ) / -26.16446124763705 + 606

  ###7.5 测试
    CAD图：(5653, 1039)
    2D图：(251,595)
    x = ( 5653 - 962 ) / 26.225806451612904 + 72 = 250.86961869618696
    y = 606 - ( 1039 - 714 ) / 26.16446124763705 = 593.5785709125063

    CAD图：(9657,8339)
    2D图：(403,315)
    x = ( 9657 - 962 ) / 26.225806451612904 + 72 = 403.54366543665435
    y = 606 - ( 8339 - 714 ) / 26.16446124763705 = 314.57416371649447



**API**

    /**
     * @description 在地图上标记位置
     * @param position {{cmd: number, id: string, x: number, y: number}}
     */
    LocationMap.markPosition( position )
 
    /*
     * @description 根据id获取人员信息
     * @param id {String}
     * @return { {id:String, name: String, type: String} }
     */
    LocationMap.getPersonInfoById( id )

**缩放与拖拽**

 * 滚动鼠标滚轮可以缩放画布元素
 * 按住鼠标左键不动可拖拽画布的位置 

## 8. 使用

参考：`dist/index_vertical_2.html`

    <!-- 依赖 jQuery -->
    <script src="../dep/jquery/2.2.4/jquery.js"></script>
    <!-- 核心文件 -->
    <script src="./asset/js/locationMap.js"></script>
    
    <div class="container" style="width: 760px; height: 1080px; background: gray;">
    
        <!--
            【style="width: 650px; height: 1920px;"】设置画布的尺寸
            【data-toggle="LocationMap"】标记为“LocationMap”，当页面DOM树构建完毕后 会对其进行初始化。
            【data-options】设置参数
            【webSocket_url】WebSocket，需要连接的URL
            【webSocket_onOpen】WebSocket，建立连接后的回调函数（名称）
            【webSocket_onMessage】WebSocket，接收到消息后的回调函数（名称）
            【webSocket_onClose】WebSocket，关闭连接后的回调函数（名称）
            【webSocket_onError】WebSocket，连接出错后的回调函数（名称）
            【personInfoListUrl】请求 人员信息列表 的URL
            【onClickPerson】点击人员位置标签后的回调函数（名称）
            【resourcesDir】资源目录
            【positionConverter】转换器，将服务器推送过来的坐标进行转换的函数（名称）
        -->
        <div style="width: 650px; height: 1920px;"
             data-toggle="LocationMap"
             data-options='{
                    "webSocket_url": "ws://localhost:8080/pkui/noauth/websocket/getPosition",
                    "webSocket_onOpen": "establishSocketCallback",
                    "webSocket_onMessage": "receivedMessageCallback",
                    "webSocket_onClose": "closeSocketCallback",
                    "webSocket_onError": "socketErrorCallback",
                    "personInfoListUrl": "../test/personInfoListData.json",
                    "onClickPerson": "clickPersonCallback",
                    "resourcesDir": "./asset/",
                    "positionConverter": "positionConverter"
                 }'
    
        >
        </div>
    
    </div>
    
    <script>
    
        /**
         * @description CAD图坐标 转 2D坐标
         * @param pos
         * @return {{x: number, y: number}}
         */
        function positionConverter ( pos ) {
            var
                cad_x = pos.x,
                cad_y = pos.y,
                x,
                y
            ;
    
            x = ( cad_x - 962 ) / 26.225806451612904 + 72;
            y = ( cad_y - 714 ) / -26.16446124763705 + 606;
    
            return {
                x: y,
                y: -(x -1920)
            };
        }
    
        /**
         * @description 连接 web socket 后，服务器发送数据后的回调函数
         * @param event
         */
        function receivedMessageCallback( event ) {
    
        }
    
        /**
         * @description 点击 位置标签后 的回调函数
         * @param position {{id: string, name: string, x: number, y: number}}
         * @param position.id {{string}} 人员ID
         * @param position.name {{string}} 人员名称
         * @param position.x {{number}} 服务器返回的X坐标
         * @param position.y {{number}} 服务器返回的Y坐标
         */
        function clickPersonCallback( position ) {
            console.info( position );
        }
    
        /**
         * @description web socket 连接出错后的回调函数
         */
        function socketErrorCallback() {
    
            LocationMap.markPosition( { cmd: 2, id: "56780", x: 24824, y: 8269 } );
            LocationMap.markPosition( { cmd: 2, id: "56781", x: 2940, y: 12564 } );
            LocationMap.markPosition( { cmd: 2, id: "56782", x: 32630, y: 13000 } );
            LocationMap.markPosition( { cmd: 2, id: "56783", x: 33518, y: 2137 } );
        }
    
    
    </script> 
 


## 9. 补充

  PkuLoc定位服务 cmd值释义
    ###0, 信号丢失 {id,regionId,regionName,regionCam}
    ###1, 信号恢复 {id,x,y,regionId,regionName,regionCam}
    ###2, 坐标更新 {id,x,y}
    ###3, 激活 {id}
    ###4, 取消激活 {id}
    ###5, 进入区域 {tagId,regionId,regionName,regionCam,ioType,tagStat}
    ###6, 离开区域 {tagId,regionId,regionName,regionCam,ioType,tagStat}
    ###7, 电量低于20% {id}
    ###8, 电量恢复正常 {id}

tip:CAD图的尺寸精度为mm（毫米），服务器推送过来的坐标精度为cm（厘米）。
所以转换器里的坐标数值 应该 除以10。



