<template>
  <div>
    <canvas id="canvasTwo">
    </canvas>
    <button id="cv" class="test">I am Now visible!</button>
  </div>
</template>

<script>
  export default {
    data: function(){
      return {
        gameVictory: false
      }
    },
    methods: {
      updateCanvas: function () {
        var canvas = document.getElementById('canvasTwo'),
          ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth - 50;
        canvas.height = 600;
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = "black";
        /*var GV = false;
        function poop() {
          console.log('ggfkjgh');
          //this.updateGameVictory();
        };*/
        //rectangle
        //ctx.fillStyle = "rgba(255, 0, 0, 0.1)";
        /*       ctx.fillRect(100, 100, 100, 100);
        ctx.font="20px Georgia";

        // Text
        //ctx.fillStyle = "rgba(0, 255, 0, 0.1)";
        ctx.fillText(this.exampleContent,10,50);

        //line
        ctx.beginPath();
        ctx.moveTo(50, 300);
        ctx.lineTo(300, 100);
        ctx.lineTo(400, 300);
        ctx.strokeStyle = "#987242";
        ctx.stroke();

        //arc, circle
       // ctx.fillStyle = "black";
        for (var i = 0; i < 100; i++) {
          var x = Math.random();
          var y = Math.random();
          console.log(r, g, b);

          ctx.beginPath();
          ctx.arc(x * 800,y * 600, 30, 0, Math.PI * 2, false);
          ctx.strokeStyle = "blue";
          ctx.stroke();
          ctx.arc
        }
        ctx.beginPath();
        ctx.arc(300, 300, 30, 0, Math.PI * 2, false);
        ctx.strokeStyle = "blue";
        ctx.stroke();*/
        var colorArray = [
          "#7CFC00",
          "#7FFF00",
          "#32CD32",
          "#00FF00",
          "#228B22",
          "#008000",
          "#006400",
          "#ADFF2F",
          "#9ACD32",
          "#00FF7F",
          "#00FA9A",
          "#90EE90",
          "#98FB98",
          "#8FBC8F",
          "#3CB371",
          "#20B2AA",
          "#2E8B57",
          "#808000",
          "#556B2F"
        ];

        var gravity = 1;
        var mu = 0;

        var mouse = {
          x: undefined,
          y: undefined
        };

        canvas.addEventListener('mousemove',
          function (event) {
            mouse.x = event.x;
            mouse.y = event.y;
          }
        );
        /* var gamePlayActive = false;
        canvas.addEventListener('mouseenter mousestay', function() {
            gamePlayActive = true;
            console.log('bananass');
          letsGo();
        });
        canvas.addEventListener('mouseleave', function() {
            gamePlayActive = false;
        });
*/
        var keyUp = true;

        function disableKey() {
          console.log(keyUp);
          keyUp = false;
          console.log(keyUp);
          setTimeout(function () {
            keyUp = true;
            console.log(keyUp);
          }, 600)
        }

        var jumping;
        window.addEventListener('keydown', function (event) {
          console.log(event);
          if (event.key === "ArrowRight" || event.key === "d") {
            ball.dx = 5;
          }
          else if (event.key === "ArrowLeft" || event.key === "a") {
            ball.dx = -5;
          }
          else if (event.key === "ArrowUp" && grounded) {
            event.preventDefault();
            grounded = false;
            jumping = true;
            disableKey();
          }
          else if (event.key === "ArrowUp" && !grounded) {
            event.preventDefault();
          }
          else if (event.key === "w" && keyUp) {
            event.preventDefault();
            ball.dy += 100;
            disableKey();
          }
        });
        window.addEventListener('keyup', function (event) {
          console.log(event);
          if (event.key === "ArrowRight" || event.key === "d") {
            ball.dx = 0;
          }
          else if (event.key === "ArrowLeft" || event.key === "a") {
            ball.dx = 0;
            console.log('pizza');
          }
        });


        window.addEventListener('resize',
          function () {
            canvas.width = window.innerWidth;
            init();
          });
        var grounded;
        var base;
        var platform = [];

        function groundCheck() {
          base = getDistance(ball.x, (ball.y + (ball.height / 2)), ball.x, canvas.height);
          /*for (var i =0; i < platformArray.length; i++){
            if (platformArray[i].x <ball.x < (platformArray[i].x + platformArray[i].width &&
            getDistance(ball.x, (ball.y + ball.height/2), ball.x, platformArray[i].y) <= 0.1)){
              ball.dy = 0;
              ball.y = platform[i].y;
              grounded = true;
            }
          }*/
           if (base <= 0.7 && !jumping || (ball.y + (ball.height / 2)) > canvas.height) {
            jumping = false;
            grounded = true;
            ball.y = canvas.height - (ball.height / 2);
          }
          else {
            grounded = false;
          }
        }


        /*var platform = false;
        function groundCheck() {
          var base = getDistance(ball.x, ball.y, ball.x, canvas.height);
          var onGround = false;
            for (var i = 0; i < platformArray.length; i++) {
              if (platformArray[i].x <= ball.x && ball.x <= (platformArray[i].x + platformArray[i].width) &&
                //getDistance(ball.x, (ball.y + (ball.height / 2)), ball.x, platformArray[i].y) <= (ball.height / 2)){
                (ball.y + ball.height / 2) - platformArray[i].y <= 0.1){
              platform = true;
                console.log(i, platformArray[i].y);
              } else {
                platform = false;
              }
            }*/
        /*if (base <= ball.radius) {
            //console.log("touching base");
          }
          else if (platform && ball.dy >= 0){
             // console.log("touching platform");
          //    ball.dy = -ball.dy;
             // gravity = 0;
          }*/
        var skills = 0;

        function handlePowerUps() {
          //this.GV = GV;
          for (var i = 0; i < powerUpArray.length; i++) {
            if (getDistance(ball.x, ball.y, powerUpArray[i].x, powerUpArray[i].y) <= (ball.width / 2) + 40 && powerUpArray[i].visi >= 0.5) {
              powerUpArray[i].visi = 0;
              skills += 1;
              console.log('COLLISION!')
            } else if (skills >= powerUpArray.length) {
              //console.log('Glorius Testicles!');
//              this.gameVictory = true;
              document.getElementById('cv').removeAttribute("class");
            }
          }
        }

        function getDistance(x1, y1, x2, y2) {
          let xDistance = x2 - x1;
          let yDistance = y2 - y1;

          return Math.sqrt(Math.pow(xDistance, 2) + Math.pow(yDistance, 2))
        };
        let walkOne = undefined;

        function Player(x, y, dy, dx, height, width) {
          //this.x0 = x0;
          //var x = (x0 - 42);
          this.x = x;
          var centerX = x + 42;
          this.centerX = centerX;
          //this.y0 = y0;
          //var y = (y0 + 18);
          var centerY = y - 18;
          this.centerY = centerY;
          this.y = y;
          this.dy = dy;
          this.dx = dx;
          this.height = height;
          this.width = width;
          this.update = function () {
            if (grounded) {
            //(this.y + (this.height / 2) + this.dy >= canvas.height || platform) {
              this.dy = -this.dy * mu;
            } else if (jumping) {
              ball.dy -= 20;
              jumping = false;
            } else {
              this.dy += gravity;
            }
            this.y += this.dy;
            this.draw();

            if (this.x + (this.width / 2) + this.dx > canvas.width || this.x - (this.width / 2) + this.dx <= 0) {
              this.dx = 0;
            }
            this.x += this.dx;
            //this.dx = this.dx * mu;
            this.draw();
          };
          /*this.move = function(velocity) {
              this.dx += velocity;
              this.draw();
          };
          this.jump = function() {
            this.dy = 100;
            this.draw();
          };*/
          this.draw = function () {


            if (this.dx === 0 && Math.abs(this.dy) <= 2) {
              ctx.drawImage(playerStand, (this.x - 42), (this.y + 18) - 60);
            }
            else if (Math.abs(this.dy) > 0.5) {
              if (this.dx >= 0) {
                ctx.drawImage(playerJump, (this.x - 42), (this.y + 18) - 60);
              } else {
                ctx.drawImage(playerJumpLeft, (this.x - 42), (this.y + 18) - 60);
              }
            }
            else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && walkOne) {
              if (this.dx >= 0) {
                ctx.drawImage(playerWalkOne, (this.x - 42), (this.y + 18) - 60);
              } else {
                ctx.drawImage(playerWalkOneLeft, (this.x - 42), (this.y + 18) - 60);
              }
              setTimeout(function () {
                walkOne = false;
              }, 70);
            }
            else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && !walkOne) {
              if (this.dx >= 0) {
                ctx.drawImage(playerWalkTwo, (this.x - 42), (this.y + 18) - 60);
              } else {
                ctx.drawImage(playerWalkTwoLeft, (this.x - 42), (this.y + 18) - 60);
              }
              setTimeout(function () {
                walkOne = true;
              }, 70);
            }
          };
        }

        function PowerUp(x, y, image, visi) {
          this.x = x;
          this.y = y;
          this.image = image;
          this.visi = visi;

          this.draw = function () {
            ctx.drawImage(powerUpImageArray[this.image], this.x, this.y, 40, 40);
            //console.log(visi);
          };
        }

        function Platform(x, y, width, text, color) {
          this.x = x;
          this.y = y;
          this.width = width;
          this.text = text;
          this.color = color;

          this.draw = function () {
            ctx.font = "30px Cinzel";
            ctx.fillStyle = "forestgreen";
            ctx.fillText(this.text, this.x, this.y);

            ctx.beginPath();
            ctx.arc(this.x, this.y, 5, 0, Math.PI * 2, false);
            ctx.fillStyle = this.color;
            ctx.fill();
            ctx.closePath();

            ctx.beginPath();
            ctx.arc(this.x + this.width, this.y, 5, 0, Math.PI * 2, false);
            ctx.fillStyle = this.color;
            ctx.fill();
            ctx.closePath();
          };
        }

        var playerStand;

        function make_base() {
          playerStand = new Image();
          playerStand.src = '../../static/Ozge1.png';
        }

        var playerWalkOne;

        function make_baseOne() {
          playerWalkOne = new Image();
          playerWalkOne.src = '../../static/OzgeRunOne.png';
        }

        var playerWalkOneLeft;

        function make_baseOneA() {
          playerWalkOneLeft = new Image();
          playerWalkOneLeft.src = '../../static/OzgeRunOneLeft.png';
        }

        var playerWalkTwo;

        function make_baseTwo() {
          playerWalkTwo = new Image();
          playerWalkTwo.src = '../../static/OzgeRunTwo.png';
        }

        var playerWalkTwoLeft;

        function make_baseTwoA() {
          playerWalkTwoLeft = new Image();
          playerWalkTwoLeft.src = '../../static/OzgeRunTwoLeft.png';
        }

        var playerJump;

        function make_baseThree() {
          playerJump = new Image();
          playerJump.src = '../../static/OzgeJump2.png';
        }

        var playerJumpLeft;

        function make_baseThreeA() {
          playerJumpLeft = new Image();
          playerJumpLeft.src = '../../static/OzgeJump2Left.png';
        }

        var powerUpImageArray = [];

        function make_powerUps() {
          var HTMl = new Image();
          HTMl.src = '../../static/html5-with-wordmark-color.svg';
          powerUpImageArray.push(HTMl);
          var javaScript = new Image();
          javaScript.src = '../../static/javascript.svg';
          powerUpImageArray.push(javaScript);
        }


        var ball;
        //var powerUp;
        //var textOne;
        var powerUpArray = [];
        var platformArray = [];

        function init() {
          powerUpArray = [];
          platformArray = [];
          ball = new Player(canvas.width / 2, canvas.height - 42, 0, 0, 84, 64);
          powerUpArray.push(new PowerUp(100, 500, 0, 1));
          powerUpArray.push(new PowerUp(200, 500, 1, 1));
          platformArray.push(new Platform(100, 300, 585, "HTML canvas games using javascript", colorArray[1]));
          platformArray.push(new Platform(520, 500, 330, "C# Games with Unity", colorArray[1]));
          make_base();
          make_baseOne();
          make_baseTwo();
          make_baseThree();
          make_baseOneA();
          make_baseTwoA();
          make_baseThreeA();
          make_powerUps();
          ctx.fillText('Skills: ' + skills, canvas.width - 150, 15);
        }

        function animate() {
          requestAnimationFrame(animate);
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          groundCheck();
          ball.update();
          for (var i = 0; i < powerUpArray.length; i++) {
            if (powerUpArray[i].visi >= 0.99) {
              powerUpArray[i].draw();
            }
          }
          handlePowerUps();
          for (var j = 0; j < platformArray.length; j++) {
            platformArray[j].draw();
          }
          ctx.font = "22px Times New Roman";
          ctx.fillStyle = "black";
          ctx.fillText('Skills: ' + skills, canvas.width - 150, 15);
          //if (getDistance(ball.x, ball.y, powerUp.x, powerUp.y) < (ball.radius + powerUp.radius)) {
          //  ball.color = 'purple';
          //} else {
          //  ball.color = colorArray[6];
          //}
          //console.log(getDistance(ball.x, ball.y, powerUp.x, powerUp.y))
          //console.log(jumping, grounded, base)
          console.log(jumping);
        }

        init();
        animate();
      }
    },
    watch: {
      exampleContent: function (val, oldVal) {
        this.updateCanvas();
      }
    },
    mounted: function () {
      this.updateCanvas();
    }
  };
</script>

<style>
  .test {
    display: none;
  }
</style>
