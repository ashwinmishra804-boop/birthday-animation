# <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
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
<script src="file/function.js" charset="utf-8"></script>
<script src="file/love.js" charset="utf-8"></script>

<script>
function playAudio(){
    var audio = document.getElementById("myAudio");
    audio.play();
}
</script>
</head>

<body>
<div id="main">

<audio autoplay="autoplay" id="myAudio">
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
            <span class="say">Keep this animation secret </span><br>
            <span class="say">Once again Happy Birthday 🎉</span><br>
        </div>
    </div>

    <div id="clock-box">
        <span id="clock"></span>
    </div>

    <canvas id="canvas" width="1100" height="680"></canvas>

</div>
</div>

<script>
(function(){
    var canvas = $('#canvas');

    if (!canvas[0].getContext) return;

    var width = canvas.width();
    var height = canvas.height();

    var opts = {
        seed: {
            x: width / 2 - 20,
            color: "rgb(190, 26, 37)",
            scale: 2
        },
        branch: [
            [535,680,570,250,500,200,30,100,[
                [540,500,455,417,340,400,13,100],
                [550,445,600,356,680,345,12,100],
                [539,281,537,248,534,217,3,40],
                [546,397,413,247,328,244,9,80],
                [546,357,608,252,678,221,6,100]
            ]]
        ],
        bloom: {
            num: 700,
            width: 1080,
            height: 650
        },
        footer: {
            width: 1200,
            height: 5,
            speed: 10
        }
    };

    var tree = new Tree(canvas[0], width, height, opts);
    var seed = tree.seed;
    var foot = tree.footer;
    var hold = 1;

    canvas.click(function(e){
        playAudio();
        var offset = canvas.offset();
        var x = e.pageX - offset.left;
        var y = e.pageY - offset.top;
        if(seed.hover(x,y)){
            hold = 0;
            canvas.unbind("click");
        }
    });

    var seedAnimate = eval(Jscex.compile("async", function () {
        seed.draw();
        while (hold) {
            $await(Jscex.Async.sleep(10));
        }
        while (seed.canScale()) {
            seed.scale(0.95);
            $await(Jscex.Async.sleep(10));
        }
        while (seed.canMove()) {
            seed.move(0, 2);
            foot.draw();
            $await(Jscex.Async.sleep(10));
        }
    }));

    var growAnimate = eval(Jscex.compile("async", function () {
        do {
            tree.grow();
            $await(Jscex.Async.sleep(10));
        } while (tree.canGrow());
    }));

    var flowAnimate = eval(Jscex.compile("async", function () {
        do {
            tree.flower(2);
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
