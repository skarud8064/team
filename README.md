<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <title> Game </title>
</head>

<body>
    <canvas id=myCanvas width=300 height=300 style="background:url('https://s2js.com/Nam/back.JPG'); background-size: cover;">
    </canvas>
    <script>
        var ctx = myCanvas.getContext("2d");
        var bug_x = 0;
        var bug_y = 0;
        var BugImg = new Image();
        BugImg.src = "https://s2js.com/Nam/basket.png";
        var melon_x = 0;
        var melon_y = 0;
        var MelonImg = new Image();
        MelonImg.src = "https://s2js.com/img/etc/watermelon2.png";
        var score = 0;
        function ImagesTouching(x1, y1, img1, x2, y2, img2) {
            if (x1 >= x2 + img2.width || x1 + img1.width <= x2)
                return false;
            if (y1 >= y2 + img2.height || y1 + img1.height <= y2)
                return false;
            return true;
        }
        
        var melon_speed = 3;
        var FPS = 40;
        var time_remaining = 20;
        function restart_game(){
            time_remaining = 20;
            score = 0;
            melon_speed = 3;
        }
        function Do_a_Frame() {
            ctx.clearRect(0, 0, myCanvas.width, myCanvas.height);
            ctx.fillStyle = "purple";
            ctx.font = "20px Arial";
            ctx.fillText("Score:" + score, 0, 20);
            bug_y = myCanvas.height - BugImg.height;
            //ctx.clearRect(0, 0, myCanvas.width, myCanvas.height);
            ctx.drawImage(BugImg, bug_x, bug_y);
            ctx.fillText("Time Remaining :" + Math.round(time_remaining), 0, 45);
            if (time_remaining <= 0) {
                ctx.fillStyle = "red";
                ctx.font = "bold 50px Arial";
                ctx.textAlingn = "center";
                ctx.fillText("Game Over", myCanvas.width / 2, myCanvas.height / 2);
                ctx.font = "bold 20px Arial";
                ctx.fillText("Press s to play again", myCanvas.width / 2, (myCanvas.height / 2) + 50);
                ctx.textAlingn = "left";
            } else {
                time_remaining = time_remaining - 1 / FPS;
                melon_y = melon_y + melon_speed;
                if (melon_y > myCanvas.height) {
                    melon_y = 0;
                    melon_x = Math.random() * (myCanvas.width - MelonImg.width);
                }
            }
            //melon_y = melon_y + 3;
            //if (melon_y > myCanvas.height) {
            //    melon_y = 0;
            //    melon_x = Math.random() * (myCanvas.width - MelonImg.width);
            //}
            ctx.drawImage(MelonImg, melon_x, melon_y);
            if (ImagesTouching(bug_x, bug_y, BugImg,
                melon_x, melon_y, MelonImg)) {
                score = score + 1;
                melon_speed = melon_speed + 0.5;
                melon_x = Math.random() * (myCanvas.width - MelonImg.width);
                melon_y = 0;
            }
            if (melon_y + MelonImg.width >= myCanvas.height){
                melon_y = 0;
                melon_x = Math.random() * (myCanvas.width - MelonImg.width);
                if (score > 0) score -= 1;

            }
        }   
        function MyKeyDownHandler(MyEvent) {
            if (MyEvent.keyCode == 37 && bug_x > 0) { bug_x = bug_x - 10; }
            if (MyEvent.keyCode == 39 && BugImg.width < myCanvas.width) { bug_x = bug_x + 10; }
            if (MyEvent.keyCode == 83 ) restart_game();
            MyEvent.preventDefault();
        
        
        }
        addEventListener("keydown", MyKeyDownHandler);
        setInterval(Do_a_Frame, 1000/FPS);
        myCanvas.width = window.innerWidth - 20;
        myCanvas.height = window.innerHeight - 20;
    </script>
</body>

</html>
