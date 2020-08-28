var anchorGlobal = {
    'fileObject': {
        'root': {},
        'media': {},
        'video': {},
    },
    'canvas': {                                 //表示画布信息
        'id': 'anchorCanvas',                   //表示画布的默认名称
        'object': '',                           //表示画布的元素对象
        'scale': {
            'width': 0.0,
            'height': 0.0
        },
        'stroke': {
            'style': '#ff0000',
            'lineWidth': 3
        },
        'showWidth': 0.0,
        'showHeight': 0.0
    }
};

/** 获取文件中的视频对象
 * @method getFileObjectVideo
 * @for anchor_show.js
 * @return 无
*/
function getFileObjectVideo() {
    return anchorGlobal.fileObject.video;
}

/** 获取文件中的视频帧率数值
 * @method getFileOjbectVideoFrameRate
 * @for anchor_show.js
 * @return 无
*/
function getFileOjbectVideoFrameRate() {
    return anchorGlobal.fileObject.video.fps;
}

/** 获取canvas对象的id
 * @method getCanvasId
 * @for anchor_show.js
 * @return 无
*/
function getCanvasId() {
    return anchorGlobal.canvas.id;
}

/** 获取canvas对象
 * @method getCanvasObject
 * @for anchor_show.js
 * @return 无
*/
function getCanvasObject() {
    return anchorGlobal.canvas.object;
}

/** 设置canvas对象
 * @method setCanvasObject
 * @for anchor_show.js
 * @return 无
*/
function setCanvasObject(element) {
    anchorGlobal.canvas.object = element;
}

/** 设置当前的宽高缩放比例数值
 * @method setCanvasScale
 * @for anchor_show.js
 * @return 无
*/
function setCanvasScale(width, height) {
    anchorGlobal.canvas.scale.width = width;
    anchorGlobal.canvas.scale.height = height;
}

/** 获取当前的宽高缩放比例数值
 * @method getCanvasScale
 * @for anchor_show.js
 * @return 无
*/
function getCanvasScale() {
    return anchorGlobal.canvas.scale;
}

/** 设置当前显示的窗口大小数值
 * @method setCanvasShowSize
 * @for anchor_show.js
 * @return 无
*/
function setCanvasShowSize(width, height) {
    anchorGlobal.canvas.showWidth = width;
    anchorGlobal.canvas.showHeight = height;
}

/** 获取当前显示的窗口大小数值
 * @method getCanvasShowSize
 * @for anchor_show.js
 * @return 无
*/
function getCanvasShowSize(width, height) {
    return {
        'width': anchorGlobal.canvas.showWidth,
        'height': anchorGlobal.canvas.showHeight,
    };
}

/** 获取需要画的标记的边框属性
 * @method getRectangleStroke
 * @for anchor_show.js
 * @return 无
*/
function getRectangleStroke() {
    return anchorGlobal.canvas.stroke;
}

/** 判断canvas对象是否存在
 * @method isCanvasObjectExist
 * @for anchor_show.js
 * @return 无
*/
function isCanvasObjectExist() {
    if (getCanvasObject() == {} || !getCanvasObject()) {
        return false;
    }

    return true;
}

/** 将
 * @method getCanvasId
 * @for anchor_show.js
 * @return 无
*/
function converFileRectangle(rectangle) {
    return {
        'x': rectangle[0],
        'y': rectangle[1],
        'width': rectangle[2] - rectangle[0],
        'height': rectangle[3] - rectangle[1],
    };
}

/** 加载锚记描述文件
 * @method loadAnchorFile
 * @for anchor_show.js
 * @param{string}file 需要下载的锚记文件位置
 * @return {null} 返回值说明
*/
function loadAnchorFile(file) {
    //写法稍微复杂，同步请求
    $.ajax({
        type: "get",//请求方式
        url: file,//地址，就是json文件的请求路径
        dataType: "json",//数据类型可以为 text xml json  script  jsonp
        async: false,
        success: function (data) {//返回的参数就是 action里面所有的有get和set方法的参数
            anchorGlobal.fileObject.root = data;
            anchorGlobal.fileObject.media = anchorGlobal.fileObject.root.media;
            anchorGlobal.fileObject.video = anchorGlobal.fileObject.media[0].video;
            setCanvasObject(null);
        }
    });
}

/** 表示初始化canvas对象的属性，以及保存部分系统数据
 * @method initCanvasElement
 * @for anchor_show.js
 * @param{number}frameNo 表示当前的帧编号
 * @return {boolen} 返回值说明
 *      true : 存在需要处理的锚记信息
 *      false: 不存在需要处理的锚记信息
*/
function initCanvasElement(containerElement, videoElement, elementId) {

    if (isCanvasObjectExist()) {
        return false;
    }

    let tmpElement = document.getElementById(elementId);
    let tmpShowSize;

    //设置显示宽高数值
    setCanvasShowSize(videoElement.clientWidth, videoElement.clientHeight);

    //设置dom 的宽高数据
    tmpShowSize = getCanvasShowSize();
    tmpElement.setAttribute('width', tmpShowSize.width);
    tmpElement.setAttribute('height', tmpShowSize.height);

    //设置宽高的缩放比率
    setCanvasScale(tmpShowSize.width / videoElement.videoWidth
            , tmpShowSize.height / videoElement.videoHeight);

    setCanvasObject(tmpElement);

    return true;
}

/** 根据时间判断是否有需要处理的锚记
 * @method drawAnchorRectangleByFrameNoOnCanvas
 * @for anchor_show.js
 * @param{number}frameNo 表示当前的帧编号
 * @return {boolen} 返回值说明
 *      true : 存在需要处理的锚记信息
 *      false: 不存在需要处理的锚记信息
*/
function drawAnchorRectangleByFrameNoOnCanvas(frameNo) {

    let tmpRectangleElement = null;
    let tmpFrameNo = frameNo;


    if (!isCanvasObjectExist()) {
        return;
    }

    //这里进四舍五入取整
    tmpFrameNo = Math.round(tmpFrameNo);

    //console.log("process frame no:" + tmpFrameNo);
    //从锚记文件对象中查找，指定时间对应的锚记信息
    for (let anchor of getFileObjectVideo().anchors) {
        if (anchor.frameNo == tmpFrameNo) {
            tmpRectangleElement = anchor;
            break;
        }
    }

    //console.log("ready show frame no:" + tmpFrameNo);
    //如果没有锚记对象，则说明没有需要信息的标记信息。直接返回即可
    if (null == tmpRectangleElement) {
        //清空之前显示的矩形框
        ClearCanvasAnchorRectangleArray();
        return false;
    }
    else {
        //如果存在，则需要显示处理
        return ShowCanvasAnchorRectangleEx(tmpRectangleElement);
    }
    return false;
}

/** 清空canva画布
 * @method ClearCanvasAnchorRectangleArray
 * @for anchor_show.js
 * @return 无
*/
function ClearCanvasAnchorRectangleArray() {
    getCanvasObject().getContext("2d")
        .clearRect(0, 0, getCanvasShowSize().width, getCanvasShowSize().height);
}

/** 根据时间判断是否有需要处理的锚记
 * @method ShowCanvasAnchorRectangleEx
 * @for anchor_show.js
 * @param{array}rectangleElement 表示需要绘制的矩形信息
 * @return 无
*/
function ShowCanvasAnchorRectangleEx(rectangleElement) {

    let tmpRectangle;
    let context = getCanvasObject().getContext("2d");


    //console.time("process time：");

    //设置矩形边框的格式
    context.strokeStyle = getRectangleStroke().style;
    context.lineWidth = getRectangleStroke().lineWidth;

    //清除当前的界面内容
    context.clearRect(0, 0, getCanvasShowSize().width, getCanvasShowSize().height);

    //绘制新的标记矩形
    for (let rectangle of rectangleElement.rectangles) {

        //转换矩形信息
        tmpRectangle = converFileRectangle(rectangle);

        //画一个矩形
        context.strokeRect(tmpRectangle.x * getCanvasScale().width
            , tmpRectangle.y * getCanvasScale().height
            , tmpRectangle.width * getCanvasScale().width
            , tmpRectangle.height * getCanvasScale().height);    //红色边框矩形
    }

    //console.timeEnd("process time：");
}
