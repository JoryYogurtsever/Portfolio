<template>
  <div class="canvasHolder">
    <canvas id="canvasOne" class="almostTooSneaky">
    </canvas>
    <canvas id="canvasTwo" class="sneaky">
    </canvas>
    <div id="cv" class="resumeHolder">
      <a href="joryHagenCV.pdf>"><button class="resumeButton">My Gorgeous CV!</button></a>
    </div>
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
        var canvas = document.getElementById('canvasOne'),
          ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth - 15;
        canvas.height = 600;
        ctx.clearRect(0, 0, canvas.width, canvas.height);
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
        var waitToTest;
        function WaitForJump() {
          setTimeout(function(){
            waitToTest = false;
          }, 200)
        }
        var clientX, clientY;
        canvas.addEventListener('touchstart', function(e) {
          // Cache the client X/Y coordinates
          e.preventDefault();
          clientX = e.touches[0].clientX;
          clientY = e.touches[0].clientY;
          console.log(clientX, clientY);
          if (clientX > ball.x) {
            ball.dx = 5;
          } else if (clientX < ball.x) {
            ball.dx = -5;
          }
        }, false);

        canvas.addEventListener('touchend', function(e) {
          var deltaX, deltaY;
          //ball.dx = 0;
          // Compute the change in X and Y coordinates.
          // The first touch point in the changedTouches
          // list is the touch point that was just removed from the surface.
          deltaX = e.changedTouches[0].clientX - clientX;
          deltaY = e.changedTouches[0].clientY - clientY;
          console.log(deltaY);
          if (deltaY < -10) {
            grounded = false;
            platform = false;
            ball.jump();
            waitToTest = true;
            WaitForJump();
          }
          if (deltaX > 0) {
            ball.dx = 5;
            setTimeout(function () {
              ball.dx = 0
            }, 300);
          }
          if (deltaX < 0) {
            ball.dx = -5;
            setTimeout(function () {
              ball.dx = 0
            }, 300);
            // Process the data ...
          }
        }, false);

        window.addEventListener('keydown', function (event) {
          //console.log(event);
          if (event.key === "ArrowRight" || event.key === "d") {
            ball.dx = 5;
          }
          else if (event.key === "ArrowLeft" || event.key === "a") {
            ball.dx = -5;
          }
          else if ((event.key === " " && grounded) || (event.key === "w" && grounded)) {
            console.log("wassup");
            event.preventDefault();
            grounded = false;
            platform = false;
            ball.jump();
            waitToTest = true;
            WaitForJump();
          }
          else if (event.key === " " && !grounded) {
            event.preventDefault();
          }
          else if (event.key === "w" && !grounded) {
            event.preventDefault();
            //ball.dy += 100;
            //disableKey();
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
        var platform;

        function groundCheck() {
          platform = false;
          base = getDistance(ball.x, (ball.y + (ball.height / 2)), ball.x, canvas.height);
          for (var i = 0; i < platformArray.length; i++) {
            if (platformArray[i].x < ball.x && ball.x < (platformArray[i].x + platformArray[i].width) &&
              (ball.y + ball.height / 2) - platformArray[i].y <= 3 &&
              getDistance(ball.x, (ball.y + ball.height/2), ball.x, platformArray[i].y) <
              platformArray[i].height * 2.2 && !waitToTest) {
              // ball.dy = 0;
              // ball.y = platform[i].y;
              grounded = true;
              console.log("brumhilda");
              platform = true;
              ball.dy = 0;
              ball.y = platformArray[i].y - platformArray[i].height * 2.1;
            }
          }
          if (platform){
            gravity = 0;
          }

            else if (base <= 0.7 || (ball.y + (ball.height / 2)) > canvas.height && !waitToTest) {
              grounded = true;
              //the line below shouldn't be in this section
              ball.y = canvas.height - (ball.height / 2);
            }
            else {
              gravity = 1;
              grounded = false;
              platform = false;
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

          this.jump = function () {
            //console.log("well then What the fuck?", grounded);
            this.dy = -18;
            this.y += this.dy;
          };
          this.update = function () {
            if (grounded) {
            //(this.y + (this.height / 2) + this.dy >= canvas.height || platform) {
              this.dy = 0;
            } else if (platform) {
              this.dy = 0;
            }
            /*} else if () {
              ball.dy -= 20;
              jumping = false;
            }*/ else if (!grounded) {
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


            if (this.dx === 0 && grounded){//Math.abs(this.dy) <= 2) {
              ctx.drawImage(playerStand, (this.x - 42), (this.y + 18) - 60);
            }
            else if (!grounded) {//(Math.abs(this.dy) > 0.5) {
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

        function PowerUp(x, y, image, width, height, visi) {
          this.x = x;
          this.y = y;
          this.width = width;
          this.height = height;
          this.image = image;
          this.visi = visi;

          this.draw = function () {
            ctx.drawImage(powerUpImageArray[this.image], this.x, this.y, this.width, this.height);
            //console.log(visi);
          };
        }

        function Platform(x, y, width, height, text, color) {
          this.x = x;
          this.y = y;
          this.width = width;
          this.height = height;
          this.text = text;
          this.color = color;

          this.draw = function () {
            ctx.font = "30px Cinzel";
            ctx.fillStyle = "forestgreen";
            ctx.fillText(this.text, this.x, this.y);

         /*   ctx.beginPath();
            ctx.arc(this.x, this.y, 5, 0, Math.PI * 2, false);
            ctx.fillStyle = this.color;
            ctx.fill();
            ctx.closePath();

            ctx.beginPath();
            ctx.arc(this.x + this.width, this.y, 5, 0, Math.PI * 2, false);
            ctx.fillStyle = this.color;
            ctx.fill();
            ctx.closePath();*/
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
          var git = new Image();
          git.src = '../../static/git-seeklogo.com.svg';
          powerUpImageArray.push(git);
          var vue = new Image();
          vue.src = '../../static/vuejs-seeklogo.com.svg';
          powerUpImageArray.push(vue);
          var css = new Image();
          css.src = '../../static/css-3-seeklogo.com.svg';
          powerUpImageArray.push(css);
          var jquery = new Image();
          jquery.src = '../../static/jquery-seeklogo.com.svg';
          powerUpImageArray.push(jquery);
          var npm = new Image();
          npm.src = '../../static/npm-node-package-manager.svg';
          powerUpImageArray.push(npm);
          var webpack = new Image();
          webpack.src = '../../static/webpack-seeklogo.com.svg';
          powerUpImageArray.push(webpack);
          var unity = new Image();
          unity.src = '../../static/unity-seeklogo.com.svg';
          powerUpImageArray.push(unity);
        }


        var ball;
        //var powerUp;
        //var textOne;
        var powerUpArray = [];
        var platformArray = [];

        function init() {
          powerUpArray = [];
          platformArray = [];
          skills = 0;
          ball = new Player(canvas.width / 2, canvas.height - 42, 0, 0, 84, 64);
          powerUpArray.push(new PowerUp(600, 250, 0, 40, 40, 1));
          powerUpArray.push(new PowerUp(320, 355, 1, 55, 55, 1));
          powerUpArray.push(new PowerUp(canvas.width - 140, 390, 2, 40, 40, 1));
          powerUpArray.push(new PowerUp((canvas.width / 3) + 55, 460, 3, 40, 40, 1));
          powerUpArray.push(new PowerUp(100, 30, 4, 30, 40, 1));
          powerUpArray.push(new PowerUp(50, 370, 5, 70, 23, 1));
          powerUpArray.push(new PowerUp(390, 45, 6, 60, 25, 1));
          powerUpArray.push(new PowerUp(200, 190, 7, 40, 40, 1));
          powerUpArray.push(new PowerUp((canvas.width *2/3) + 100, 110, 8, 40, 40, 1));
          platformArray.push(new Platform(100, 100, 350, 30, "I've build video games", colorArray[1]));
          platformArray.push(new Platform(canvas.width * 2/3, 180, 192, 30, "using Unity", colorArray[1]));
          platformArray.push(new Platform(30, 260, 192, 30, "coded in C#", colorArray[1]));
          platformArray.push(new Platform(canvas.width/4, 320, 438, 30, "I've also  used HTML canvas", colorArray[1]));
          platformArray.push(new Platform(10, 430, 389, 30, "and coded in javascript", colorArray[1]));
          platformArray.push(new Platform(canvas.width - 300, 460, 280, 30, "Use A, D and W or", colorArray[1]));
          platformArray.push(new Platform(canvas.width - 300, 510, 230, 30, "\u21E6, \u21E8 and space", colorArray[1]));
          platformArray.push(new Platform(canvas.width / 3, 535, 350, 30, "to give this one a try!", colorArray[1]));
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
          //console.log(grounded, platform);
        }

        init();
        animate();
      },
    updateCanvasTwo: function () {
      var canvas = document.getElementById('canvasTwo'),
        ctx = canvas.getContext('2d');
      canvas.width = window.innerWidth;
      canvas.height = 480;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      //ctx.fillStyle = "black";
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
      var waitToTest;
      function WaitForJump() {
        setTimeout(function(){
          waitToTest = false;
        }, 200)
      }
      var clientX, clientY;
      canvas.addEventListener('touchstart', function(e) {
        // Cache the client X/Y coordinates
        e.preventDefault();
        clientX = e.touches[0].clientX;
        clientY = e.touches[0].clientY;
        console.log(clientX, clientY);
        if (clientX > ball.x) {
          ball.dx = 3;
        } else if (clientX < ball.x) {
          ball.dx = -3;
        }
      }, false);

      canvas.addEventListener('touchend', function(e) {
        var deltaX, deltaY;
        //ball.dx = 0;
        // Compute the change in X and Y coordinates.
        // The first touch point in the changedTouches
        // list is the touch point that was just removed from the surface.
        deltaX = e.changedTouches[0].clientX - clientX;
        deltaY = e.changedTouches[0].clientY - clientY;
        console.log(deltaY);
        if (deltaY < -10) {
          grounded = false;
          platform = false;
          ball.jump();
          waitToTest = true;
          WaitForJump();
        }
        if (deltaX > 0) {
          ball.dx = 3;
          setTimeout(function () {
            ball.dx = 0
          }, 300);
        }
        if (deltaX < 0) {
          ball.dx = -3;
          setTimeout(function () {
            ball.dx = 0
          }, 300);
          // Process the data ...
        }
      }, false);

      window.addEventListener('keydown', function (event) {
        //console.log(event);
        if (event.key === "ArrowRight" || event.key === "d") {
          ball.dx = 3;
        }
        else if (event.key === "ArrowLeft" || event.key === "a") {
          ball.dx = -3;
        }
        else if ((event.key === " " && grounded) || (event.key === "w" && grounded)) {
          console.log("wassup");
          event.preventDefault();
          grounded = false;
          platform = false;
          ball.jump();
          waitToTest = true;
          WaitForJump();
        }
        else if (event.key === " " && !grounded) {
          event.preventDefault();
        }
        else if (event.key === "w" && !grounded) {
          event.preventDefault();
          //ball.dy += 100;
          //disableKey();
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
      var platform;

      function groundCheck() {
        platform = false;
        base = getDistance(ball.x, (ball.y + (ball.height / 2)), ball.x, canvas.height);
        for (var i = 0; i < platformArray.length; i++) {
          if (platformArray[i].x < ball.x && ball.x < (platformArray[i].x + platformArray[i].width) &&
            (ball.y + ball.height / 2) - platformArray[i].y <= 3 &&
            getDistance(ball.x, (ball.y + ball.height/2), ball.x, platformArray[i].y) <
            platformArray[i].height * 2.2 && !waitToTest) {
            // ball.dy = 0;
            // ball.y = platform[i].y;
            grounded = true;
            console.log("brumhilda");
            platform = true;
            ball.dy = 0;
            ball.y = platformArray[i].y - platformArray[i].height * 2.1;
          }
        }
        if (platform){
          gravity = 0;
        }

        else if (base <= 0.7 || (ball.y + (ball.height / 2)) > canvas.height && !waitToTest) {
          grounded = true;
          //the line below shouldn't be in this section
          ball.y = canvas.height - (ball.height / 2);
        }
        else {
          gravity = 1;
          grounded = false;
          platform = false;
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
          if (getDistance(ball.x - 4, ball.y, powerUpArray[i].x, powerUpArray[i].y) <= (ball.width / 2) + 8 && powerUpArray[i].visi >= 0.5) {
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

        this.jump = function () {
          //console.log("well then What the fuck?", grounded);
          this.dy = -12;
          this.y += this.dy;
        };
        this.update = function () {
          if (grounded) {
            //(this.y + (this.height / 2) + this.dy >= canvas.height || platform) {
            this.dy = 0;
          } else if (platform) {
            this.dy = 0;
          }
          /*} else if () {
            ball.dy -= 20;
            jumping = false;
          }*/ else if (!grounded) {
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
        /*  ctx.beginPath();
          ctx.arc(this.x, this.y, 2, 0, Math.PI * 2, false);
          ctx.fillStyle = 'black';
          ctx.fill();
          ctx.closePath();

          ctx.beginPath();
          ctx.arc(this.x + 16, this.y + 20, 2, 0, Math.PI * 2, false);
          ctx.fillStyle = 'black';
          ctx.fill();
          ctx.closePath();*/

          if (this.dx === 0 && grounded){//Math.abs(this.dy) <= 2) {
            ctx.drawImage(playerStand, (this.x - 16), (this.y - 18), this.width, this.height);
          }
          else if (!grounded) {//(Math.abs(this.dy) > 0.5) {
            if (this.dx >= 0) {
              ctx.drawImage(playerJump, (this.x - 16), (this.y - 18), this.width, this.height);
            } else {
              ctx.drawImage(playerJumpLeft, (this.x - 16), (this.y - 18), this.width, this.height);
            }
          }
          else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && walkOne) {
            if (this.dx >= 0) {
              ctx.drawImage(playerWalkOne, (this.x - 16), (this.y - 18), this.width, this.height);
            } else {
              ctx.drawImage(playerWalkOneLeft, (this.x - 16), (this.y - 18), this.width, this.height);
            }
            setTimeout(function () {
              walkOne = false;
            }, 70);
          }
          else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && !walkOne) {
            if (this.dx >= 0) {
              ctx.drawImage(playerWalkTwo, (this.x - 16), (this.y - 18), this.width, this.height);
            } else {
              ctx.drawImage(playerWalkTwoLeft, (this.x - 16), (this.y - 18), this.width, this.height);
            }
            setTimeout(function () {
              walkOne = true;
            }, 70);
          }
        };
      }

      function PowerUp(x, y, image, width, height, visi) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.image = image;
        this.visi = visi;

        this.draw = function () {
          ctx.drawImage(powerUpImageArray[this.image], this.x, this.y, this.width, this.height);
          //console.log(visi);
        };
      }

      function Platform(x, y, width, height, text, color) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.text = text;
        this.color = color;

        this.draw = function () {
          ctx.font = "16px Cinzel";
          ctx.fillStyle = "forestgreen";
          ctx.fillText(this.text, this.x, this.y);

          /*   ctx.beginPath();
             ctx.arc(this.x, this.y - this.height, 5, 0, Math.PI * 2, false);
             ctx.fillStyle = this.color;
             ctx.fill();
             ctx.closePath();

             ctx.beginPath();
             ctx.arc(this.x + this.width, this.y, 5, 0, Math.PI * 2, false);
             ctx.fillStyle = this.color;
             ctx.fill();
             ctx.closePath();*/
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
        var git = new Image();
        git.src = '../../static/git-seeklogo.com.svg';
        powerUpImageArray.push(git);
        var vue = new Image();
        vue.src = '../../static/vuejs-seeklogo.com.svg';
        powerUpImageArray.push(vue);
        var css = new Image();
        css.src = '../../static/css-3-seeklogo.com.svg';
        powerUpImageArray.push(css);
        var jquery = new Image();
        jquery.src = '../../static/jquery-seeklogo.com.svg';
        powerUpImageArray.push(jquery);
        var npm = new Image();
        npm.src = '../../static/npm-node-package-manager.svg';
        powerUpImageArray.push(npm);
        var webpack = new Image();
        webpack.src = '../../static/webpack-seeklogo.com.svg';
        powerUpImageArray.push(webpack);
        var unity = new Image();
        unity.src = '../../static/unity-seeklogo.com.svg';
        powerUpImageArray.push(unity);
      }


      var ball;
      //var powerUp;
      //var textOne;
      var powerUpArray = [];
      var platformArray = [];

      function init() {
        powerUpArray = [];
        platformArray = [];
        skills = 0;
        ball = new Player(canvas.width / 2, canvas.height - 18, 0, 0, 34, 34);
        powerUpArray.push(new PowerUp(230, 280, 0, 20, 20, 1));
        powerUpArray.push(new PowerUp(150, 335, 1, 25, 25, 1));
        powerUpArray.push(new PowerUp(canvas.width - 40, 378, 2, 20, 20, 1));
        powerUpArray.push(new PowerUp((canvas.width / 3) + 55, 460, 3, 20, 20, 1));
        powerUpArray.push(new PowerUp(100, 55, 4, 15, 20, 1));
        powerUpArray.push(new PowerUp(20, 340, 5, 30, 15, 1));
        powerUpArray.push(new PowerUp(190, 60, 6, 30, 15, 1));
        powerUpArray.push(new PowerUp(110, 190, 7, 20, 20, 1));
        powerUpArray.push(new PowerUp((canvas.width *2/3) + 10, 140, 8, 20, 20, 1));
        platformArray.push(new Platform(100, 100, 188, 14, "I've build video games", colorArray[1]));
        platformArray.push(new Platform(canvas.width * 1/2, 180, 102, 14, "using Unity", colorArray[1]));
        platformArray.push(new Platform(30, 230, 102, 14, "coded in C#", colorArray[1]));
        platformArray.push(new Platform(canvas.width/4, 320, 233, 14, "I've also  used HTML canvas", colorArray[1]));
        platformArray.push(new Platform(10, 380, 207, 14, "and coded in javascript", colorArray[1]));
        platformArray.push(new Platform(canvas.width - 180, 420, 163, 14, "give this one a try!", colorArray[1]));
       // platformArray.push(new Platform(canvas.width - 80, 510, 230, 30, "\u21E6, \u21E8 and space", colorArray[1]));
       // platformArray.push(new Platform(canvas.width / 3, 535, 350, 30, "to give this one a try!", colorArray[1]));
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
        //console.log(grounded, platform);
      }

      init();
      animate();
    }
  },
    watch: {
      exampleContent: function (val, oldVal) {
        this.updateCanvas();
        this.updateCanvasTwo();
      }
    },
    mounted: function () {
      this.updateCanvas();
      this.updateCanvasTwo();
    }
  };
</script>

<style>
  .resumeHolder {
    display: none;
  }
  .testTwo {
    position: absolute;
    font-size: 1.5em;
    bottom: 300px;
    left: 40%;
    width: 250px;
    height: 60px;
    background-color: seagreen;
    color: white;
    border: 2px solid rgba(219, 219, 219, 0.5);
  /*  margin-bottom: -2050px;*/
  }
  #canvasOne {
    border-top: 2px solid seagreen;
    border-bottom: 2px solid seagreen;
  }
  #canvasTwo {
    border-top: 2px solid seagreen;
    border-bottom: 2px solid seagreen;
  }
  .canvasHolder {
    position: relative;
  }
  .sneaky {
    display: none;
  }
  @media screen and (max-width:640px) {
    #canvasTwo {
      display: block;
    }
    .almostTooSneaky {
      display: none;
    }
    .resumeButton {
      font-size: 1em;
      bottom: 250px;
      left: 35%;
      width: 150px;
      height: 42px;
    }
  }
</style>
