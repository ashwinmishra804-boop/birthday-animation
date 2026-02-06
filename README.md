<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

<html xml:lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<title>HBD PRYLL</title>

<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">

<link rel="stylesheet" type="text/css" href="file/default.css" />

<script src="file/jquery.min.js"></script>
<script src="file/jscex.min.js"></script>
<script src="file/jscex-parser.js"></script>
<script src="file/jscex-jit.js"></script>
<script src="file/jscex-builderbase.min.js"></script>
<script src="file/jscex-async.min.js"></script>
<script src="file/jscex-async-powerpack.min.js"></script>
<script src="file/function.js"></script>
<script src="file/love.js"></script>

<style>
/* 🔥 MOBILE AUTO CENTER FIX */
#wrap{
    position:relative;
    width:1100px;
    height:680px;
    margin:auto;
    transform-origin: top center;
}
</style>

<script>
function playAudio(){
    document.getElementById("myAudio").play();
}

/* 📱 AUTO SCALE FOR MOBILE */
function autoCenter(){
    const wrap = document.getElementById("wrap");
    const sw = window.innerWidth;
    let scale = sw / 1100;

    if(scale > 1) scale = 1;

    wrap.style.transform = "scale(" + scale + ")";
}

window.addEventListener("load", autoCenter);
window.addEventListener("resize", autoCenter);
</script>
</head>

<body>

<div id="main">

<audio autoplay id="myAudio">
    <source src="aud.mp3" type="audio/mp3" />
</audio>

<div id="wrap">

    <div id="text">
        <div id="code">
            <span class="say">HEYY, PRYLL ✨</span><br>
            <span class="say">Happy Birthday 🎈</span><br>
            <span class="say">God bless you 🍀</span><br>
            <span class="say">Lots of happiness for you 💕</span><br>
            <span class="say">I hope u understand 🤍</span><br>
            <span class="say">Keep this animation secret</span><br>
            <span class="say">Once again Happy Birthday 🎉</span><br>
        </div>
    </div>

    <canvas id="canvas" width="1100" height="680"></canvas>
</div>
</div>

<script>
(function(){

var canvas = $('#canvas');
if (!canvas[0].getContext) return;

var ctx = canvas[0].getContext("2d");
var width = canvas.width();
var height = canvas.height();

/* 🎂 CAKE */
let cakeGrow = 0;
let flame = 0;

function drawCake(){
    if(cakeGrow < 1) cakeGrow += 0.003;

    let baseH = 120 * cakeGrow;
    let topH  = 70 * cakeGrow;
    let candleH = 50 * cakeGrow;

    ctx.fillStyle="#ffe4ec";
    ctx.fillRect(820,580-baseH,220,baseH);

    ctx.fillStyle="#ff8fab";
    ctx.fillRect(860,520-topH,140,topH);

    ctx.fillStyle="white";
    ctx.fillRect(925,470-candleH,10,candleH);

    if(cakeGrow >= 1) drawFlame();
}

function drawFlame(){
    flame += 0.3;
    let f = Math.sin(flame)*5;

    ctx.beginPath();
    ctx.arc(930,395,18,0,Math.PI*2);
    ctx.fillStyle="rgba(255,150,0,0.3)";
    ctx.fill();

    ctx.beginPath();
    ctx.fillStyle="orange";
    ctx.moveTo(930,420+f);
    ctx.quadraticCurveTo(910,380,930,350);
    ctx.quadraticCurveTo(950,380,930,420+f);
    ctx.fill();
}

/* 🌳 TREE (ORIGINAL) */
var opts = {
    seed:{ x: width/2-20, color:"rgb(190,26,37)", scale:2 },
    branch:[
        [535,680,570,250,500,200,30,100,[
            [540,500,455,417,340,400,13,100],
            [550,445,600,356,680,345,12,100],
            [539,281,537,248,534,217,3,40],
            [546,397,413,247,328,244,9,80],
            [546,357,608,252,678,221,6,100]
        ]]
    ],
    bloom:{ num:700, width:1080, height:650 }
};

var tree = new Tree(canvas[0], width, height, opts);
var seed = tree.seed;
var hold = 1;

canvas.click(function(e){
    playAudio();
    var o = canvas.offset();
    var x = e.pageX - o.left;
    var y = e.pageY - o.top;
    if(seed.hover(x,y)){
        hold = 0;
        canvas.unbind("click");
    }
});

var seedAnimate = eval(Jscex.compile("async", function () {
    seed.draw();
    while (hold) { $await(Jscex.Async.sleep(10)); }
    while (seed.canScale()) {
        seed.scale(0.95);
        $await(Jscex.Async.sleep(10));
    }
    while (seed.canMove()) {
        seed.move(0, 2);
        $await(Jscex.Async.sleep(10));
    }
}));

var growAnimate = eval(Jscex.compile("async", function () {
    do {
        tree.grow();
        drawCake();
        $await(Jscex.Async.sleep(10));
    } while (tree.canGrow());
}));

var flowAnimate = eval(Jscex.compile("async", function () {
    do {
        tree.flower(2);
        drawCake();
        $await(Jscex.Async.sleep(10));
    } while (tree.canFlower());
}));

var textAnimate = eval(Jscex.compile("async", function () {
    $("#code").show().typewriter();
}));

var runAsync = eval(Jscex.compile("async", function () {
    $await(seedAnimate());
    $await(growAnimate());
    $await(flowAnimate());
    textAnimate().start();
}));

runAsync().start();

})();
</script>

</body>
</html>
