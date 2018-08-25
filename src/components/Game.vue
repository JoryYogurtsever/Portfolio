<template>
  <div class="canvasHolder">
    <canvas id="canvasOne" class="almostTooSneaky">
    </canvas>
    <canvas id="canvasTwo" class="sneaky">
    </canvas>
    <div id="cv" class="resumeHolder">
      <a href="../../static/joryHagenCV.pdf">
        <button class="resumeButton">My Gorgeous CV!</button>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      gameVictory: false,
    };
  },
  methods: {
    updateCanvas() {
      //  Set up The Full Width Canvas
      const canvas = document.getElementById('canvasOne');
      const ctx = canvas.getContext('2d');
      //  Set a delay to prevent double jumping and glitchy behaviour
      let waitToTest;
      //  Set the gravity
      let gravity = 1;
      let clientX;
      let clientY;
      let grounded;
      let base;
      let platform;
      let skills;
      skills = 0;
      let playerStand;
      let ball;
      let powerUpArray;
      powerUpArray = [];
      let platformArray;
      platformArray = [];

      canvas.width = window.innerWidth - 15;
      canvas.height = 600;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      //  import the art for the player and power-ups
      function makeBase() {
        playerStand = new Image();
        playerStand.src = '../../static/Ozge1.png';
      }

      let playerWalkOne;

      function makeBaseOne() {
        playerWalkOne = new Image();
        playerWalkOne.src = '../../static/OzgeRunOne.png';
      }

      let playerWalkOneLeft;

      function makeBaseOneA() {
        playerWalkOneLeft = new Image();
        playerWalkOneLeft.src = '../../static/OzgeRunOneLeft.png';
      }

      let playerWalkTwo;

      function makeBaseTwo() {
        playerWalkTwo = new Image();
        playerWalkTwo.src = '../../static/OzgeRunTwo.png';
      }

      let playerWalkTwoLeft;

      function makeBaseTwoA() {
        playerWalkTwoLeft = new Image();
        playerWalkTwoLeft.src = '../../static/OzgeRunTwoLeft.png';
      }

      let playerJump;

      function makeBaseThree() {
        playerJump = new Image();
        playerJump.src = '../../static/OzgeJump2.png';
      }

      let playerJumpLeft;

      function makeBaseThreeA() {
        playerJumpLeft = new Image();
        playerJumpLeft.src = '../../static/OzgeJump2Left.png';
      }

      const powerUpImageArray = [];

      function makePowerUps() {
        const HTMl = new Image();
        HTMl.src = '../../static/html5-with-wordmark-color.svg';
        powerUpImageArray.push(HTMl);
        const javaScript = new Image();
        javaScript.src = '../../static/javascript.svg';
        powerUpImageArray.push(javaScript);
        const git = new Image();
        git.src = '../../static/git-seeklogo.com.svg';
        powerUpImageArray.push(git);
        const vue = new Image();
        vue.src = '../../static/vuejs-seeklogo.com.svg';
        powerUpImageArray.push(vue);
        const css = new Image();
        css.src = '../../static/css-3-seeklogo.com.svg';
        powerUpImageArray.push(css);
        const jquery = new Image();
        jquery.src = '../../static/jquery-seeklogo.com.svg';
        powerUpImageArray.push(jquery);
        const npm = new Image();
        npm.src = '../../static/npm-node-package-manager.svg';
        powerUpImageArray.push(npm);
        const webpack = new Image();
        webpack.src = '../../static/webpack-seeklogo.com.svg';
        powerUpImageArray.push(webpack);
        const unity = new Image();
        unity.src = '../../static/unity-seeklogo.com.svg';
        powerUpImageArray.push(unity);
      }
      let walkOne;

      //  create and animate the player
      function Player(x, y, dy, dx, height, width) {
        this.x = x;
        const centerX = x + 42;
        this.centerX = centerX;

        const centerY = y - 18;
        this.centerY = centerY;
        this.y = y;
        this.dy = dy;
        this.dx = dx;
        this.height = height;
        this.width = width;

        this.jump = () => {
          this.dy = -18;
          this.y += this.dy;
        };
        this.update = () => {
          if (grounded) {
            this.dy = 0;
          } else if (platform) {
            this.dy = 0;
          } else if (!grounded) {
            this.dy += gravity;
          }
          this.y += this.dy;
          this.draw();

          if (this.x + (this.width / 2) + this.dx > canvas.width
            || this.x - (this.width / 2) + this.dx <= 0) {
            this.dx = 0;
          }
          this.x += this.dx;
          this.draw();
        };

        this.draw = () => {
          if (this.dx === 0 && grounded) {
            ctx.drawImage(playerStand, (this.x - 42), (this.y + 18) - 60);
          } else if (!grounded) {
            if (ball.dx >= 0) {
              ctx.drawImage(playerJump, (this.x - 42), (this.y + 18) - 60);
            } else {
              ctx.drawImage(playerJumpLeft, (this.x - 42), (this.y + 18) - 60);
            }
          } else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && walkOne) {
            if (this.dx >= 0) {
              ctx.drawImage(playerWalkOne, (this.x - 42), (this.y + 18) - 60);
            } else {
              ctx.drawImage(playerWalkOneLeft, (this.x - 42), (this.y + 18) - 60);
            }
            setTimeout(() => {
              walkOne = false;
            }, 70);
          } else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && !walkOne) {
            if (this.dx >= 0) {
              ctx.drawImage(playerWalkTwo, (this.x - 42), (this.y + 18) - 60);
            } else {
              ctx.drawImage(playerWalkTwoLeft, (this.x - 42), (this.y + 18) - 60);
            }
            setTimeout(() => {
              walkOne = true;
            }, 70);
          }
        };
      }

      //  create and animate the power-ups
      function PowerUp(x, y, image, width, height, visi) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.image = image;
        this.visi = visi;

        this.draw = () => {
          ctx.drawImage(powerUpImageArray[this.image], this.x, this.y, this.width, this.height);
        };
      }

      //  create and animate the platforms
      function Platform(x, y, width, height, text) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.text = text;

        this.draw = () => {
          ctx.font = '30px Cinzel';
          ctx.fillStyle = 'forestgreen';
          ctx.fillText(this.text, this.x, this.y);
        };
      }


      //  initialize the game (called on resize as well)
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
        powerUpArray.push(new PowerUp((canvas.width * 2 / 3) + 100, 110, 8, 40, 40, 1));
        platformArray.push(new Platform(100, 100, 350, 30, 'I\'ve build video games'));
        platformArray.push(new Platform((canvas.width * 2 / 3), 180, 192, 30, 'using Unity'));
        platformArray.push(new Platform(30, 260, 192, 30, 'coded in C#'));
        platformArray.push(new Platform((canvas.width / 4), 320, 438, 30, 'I\'ve also  used HTML canvas'));
        platformArray.push(new Platform(10, 430, 389, 30, 'and coded in javascript'));
        platformArray.push(new Platform(canvas.width - 300, 460, 280, 30, 'Use A, D and W or'));
        platformArray.push(new Platform(canvas.width - 300, 510, 230, 30, '\u21E6, \u21E8 and space'));
        platformArray.push(new Platform(canvas.width / 3, 535, 350, 30, 'to give this one a try!'));
        makeBase();
        makeBaseOne();
        makeBaseTwo();
        makeBaseThree();
        makeBaseOneA();
        makeBaseTwoA();
        makeBaseThreeA();
        makePowerUps();
        ctx.fillText(`Skills: ${skills}`, canvas.width - 150, 15);
      }

      //  used to test proximity
      function getDistance(x1, y1, x2, y2) {
        const xDistance = x2 - x1;
        const yDistance = y2 - y1;

        return Math.sqrt((xDistance ** 2) + (yDistance ** 2));
      }

      function WaitForJump() {
        setTimeout(() => {
          waitToTest = false;
        }, 200);
      }

      //  Touch Screen controls for Tablets
      canvas.addEventListener('touchstart', (e) => {
        e.preventDefault();
        clientX = e.touches[0].clientX;
        clientY = e.touches[0].clientY;
        if (clientX > ball.x) {
          ball.dx = 5;
        } else if (clientX < ball.x) {
          ball.dx = -5;
        }
      }, false);

      canvas.addEventListener('touchend', (e) => {
        const deltaX = e.changedTouches[0].clientX - clientX;
        const deltaY = e.changedTouches[0].clientY - clientY;
        if (deltaY < -10) {
          grounded = false;
          platform = false;
          ball.jump();
          waitToTest = true;
          WaitForJump();
        }
        if (deltaX > 0) {
          ball.dx = 5;
          setTimeout(() => {
            ball.dx = 0;
          }, 300);
        }
        if (deltaX < 0) {
          ball.dx = -5;
          setTimeout(() => {
            ball.dx = 0;
          }, 300);
        }
      }, false);


      // Keyboard Controls for computers
      window.addEventListener('keydown', (event) => {
        if (event.key === 'ArrowRight' || event.key === 'd') {
          ball.dx = 5;
        } else if (event.key === 'ArrowLeft' || event.key === 'a') {
          ball.dx = -5;
        } else if ((event.key === ' ' && grounded) || (event.key === 'w' && grounded)) {
          event.preventDefault();
          grounded = false;
          platform = false;
          ball.jump();
          waitToTest = true;
          WaitForJump();
        } else if (event.key === ' ' && !grounded) {
          event.preventDefault();
        } else if (event.key === 'w' && !grounded) {
          event.preventDefault();
        }
      });
      window.addEventListener('keyup', (event) => {
        if (event.key === 'ArrowRight' || event.key === 'd') {
          ball.dx = 0;
        } else if (event.key === 'ArrowLeft' || event.key === 'a') {
          ball.dx = 0;
        }
      });

      //  control screen resize behaviour
      window.addEventListener('resize',
        () => {
          canvas.width = window.innerWidth;
          init();
        });

      //  determine whether the player is touching something that should behave as ground.
      function groundCheck() {
        platform = false;
        base = getDistance(ball.x, (ball.y + (ball.height / 2)), ball.x, canvas.height);
        for (let i = 0; i < platformArray.length; i += 1) {
          if (platformArray[i].x < ball.x
            && ball.x < (platformArray[i].x + platformArray[i].width)
            && (ball.y + ball.height / 2) - platformArray[i].y <= 3
            && getDistance(ball.x, (ball.y + ball.height / 2), ball.x, platformArray[i].y)
            < platformArray[i].height * 2.2 && !waitToTest) {
            grounded = true;
            platform = true;
            ball.dy = 0;
            ball.y = platformArray[i].y - platformArray[i].height * 2.1;
          }
        }
        if (platform) {
          gravity = 0;
        } else if ((base <= 0.7 || (ball.y + (ball.height / 2)) > canvas.height) && !waitToTest) {
          grounded = true;
          //  the line below shouldn't be in this section
          ball.y = canvas.height - (ball.height / 2);
        } else {
          gravity = 1;
          grounded = false;
          platform = false;
        }
      }


      // deactivate power-ups and check for victory condition

      function handlePowerUps() {
        for (let i = 0; i < powerUpArray.length; i += 1) {
          if (getDistance(ball.x, ball.y, powerUpArray[i].x, powerUpArray[i].y)
            <= (ball.width / 2) + 40
            && powerUpArray[i].visi >= 0.5) {
            powerUpArray[i].visi = 0;
            skills += 1;
          } else if (skills >= powerUpArray.length) {
            document.getElementById('cv').removeAttribute('class');
          }
        }
      }


      //  animate the canvas
      function animate() {
        requestAnimationFrame(animate);
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        groundCheck();
        ball.update();
        for (let i = 0; i < powerUpArray.length; i += 1) {
          if (powerUpArray[i].visi >= 0.99) {
            powerUpArray[i].draw();
          }
        }
        handlePowerUps();
        for (let j = 0; j < platformArray.length; j += 1) {
          platformArray[j].draw();
        }
        ctx.font = '22px Times New Roman';
        ctx.fillStyle = 'black';
        ctx.fillText(`Skills: ${skills}`, canvas.width - 150, 15);
      }

      init();
      animate();
    },
    //  I needed to write the entire game a second time to make it responsive.
    //  I couldn't find anyway to make the game attractive and practical on smaller
    //  screens without changing values that can't be passed easily into the canvas.
    updateCanvasTwo() {
    //  create the small device canvas
      const canvas = document.getElementById('canvasTwo');
      const ctx = canvas.getContext('2d');
      canvas.width = window.innerWidth;
      canvas.height = 480;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      //  set gravity
      let gravity = 1;
      //  delay jumping to prevent glitches and double jumps
      let waitToTest;
      let grounded;
      let base;
      let platform;
      //  handel the initialization of the game (called also on resize)
      let ball;
      let powerUpArray = [];
      let platformArray = [];
      //  import artwork for player and power-ups
      let playerStand;
      let skills = 0;

      function makeBase() {
        playerStand = new Image();
        playerStand.src = '../../static/Ozge1.png';
      }

      let playerWalkOne;

      function makeBaseOne() {
        playerWalkOne = new Image();
        playerWalkOne.src = '../../static/OzgeRunOne.png';
      }

      let playerWalkOneLeft;

      function makeBaseOneA() {
        playerWalkOneLeft = new Image();
        playerWalkOneLeft.src = '../../static/OzgeRunOneLeft.png';
      }

      let playerWalkTwo;

      function makeBaseTwo() {
        playerWalkTwo = new Image();
        playerWalkTwo.src = '../../static/OzgeRunTwo.png';
      }

      let playerWalkTwoLeft;

      function makeBaseTwoA() {
        playerWalkTwoLeft = new Image();
        playerWalkTwoLeft.src = '../../static/OzgeRunTwoLeft.png';
      }

      let playerJump;

      function makeBaseThree() {
        playerJump = new Image();
        playerJump.src = '../../static/OzgeJump2.png';
      }

      let playerJumpLeft;

      function makeBaseThreeA() {
        playerJumpLeft = new Image();
        playerJumpLeft.src = '../../static/OzgeJump2Left.png';
      }

      const powerUpImageArray = [];

      function makePowerUps() {
        const HTMl = new Image();
        HTMl.src = '../../static/html5-with-wordmark-color.svg';
        powerUpImageArray.push(HTMl);
        const javaScript = new Image();
        javaScript.src = '../../static/javascript.svg';
        powerUpImageArray.push(javaScript);
        const git = new Image();
        git.src = '../../static/git-seeklogo.com.svg';
        powerUpImageArray.push(git);
        const vue = new Image();
        vue.src = '../../static/vuejs-seeklogo.com.svg';
        powerUpImageArray.push(vue);
        const css = new Image();
        css.src = '../../static/css-3-seeklogo.com.svg';
        powerUpImageArray.push(css);
        const jquery = new Image();
        jquery.src = '../../static/jquery-seeklogo.com.svg';
        powerUpImageArray.push(jquery);
        const npm = new Image();
        npm.src = '../../static/npm-node-package-manager.svg';
        powerUpImageArray.push(npm);
        const webpack = new Image();
        webpack.src = '../../static/webpack-seeklogo.com.svg';
        powerUpImageArray.push(webpack);
        const unity = new Image();
        unity.src = '../../static/unity-seeklogo.com.svg';
        powerUpImageArray.push(unity);
      }

      let walkOne;
      //  control the animation and physics of the player.
      function Player(x, y, dy, dx, height, width) {
        this.x = x;
        const centerX = x + 42;
        this.centerX = centerX;
        const centerY = y - 18;
        this.centerY = centerY;
        this.y = y;
        this.dy = dy;
        this.dx = dx;
        this.height = height;
        this.width = width;

        this.jump = () => {
          this.dy = -12;
          this.y += this.dy;
        };
        this.update = () => {
          if (grounded) {
            this.dy = 0;
          } else if (platform) {
            this.dy = 0;
          } else if (!grounded) {
            this.dy += gravity;
          }
          this.y += this.dy;
          this.draw();

          if (this.x + (this.width / 2) + this.dx > canvas.width
            || this.x - (this.width / 2) + this.dx <= 0) {
            this.dx = 0;
          }
          this.x += this.dx;
          this.draw();
        };

        this.draw = () => {
          if (this.dx === 0 && grounded) {
            ctx.drawImage(playerStand, (this.x - 16), (this.y - 18), this.width, this.height);
          } else if (!grounded) {
            if (this.dx >= 0) {
              ctx.drawImage(playerJump, (this.x - 16), (this.y - 18), this.width, this.height);
            } else {
              ctx.drawImage(playerJumpLeft, (this.x - 16), (this.y - 18), this.width, this.height);
            }
          } else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && walkOne) {
            if (this.dx >= 0) {
              ctx.drawImage(playerWalkOne, (this.x - 16), (this.y - 18), this.width, this.height);
            } else {
              ctx.drawImage(playerWalkOneLeft, (this.x - 16),
                (this.y - 18), this.width, this.height);
            }
            setTimeout(() => {
              walkOne = false;
            }, 70);
          } else if (Math.abs(this.dy) <= 0.5 && Math.abs(this.dx) > 0 && !walkOne) {
            if (this.dx >= 0) {
              ctx.drawImage(playerWalkTwo, (this.x - 16), (this.y - 18), this.width, this.height);
            } else {
              ctx.drawImage(playerWalkTwoLeft, (this.x - 16),
                (this.y - 18), this.width, this.height);
            }
            setTimeout(() => {
              walkOne = true;
            }, 70);
          }
        };
      }
      //  control the animation of the powerups
      function PowerUp(x, y, image, width, height, visi) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.image = image;
        this.visi = visi;

        this.draw = () => {
          ctx.drawImage(powerUpImageArray[this.image], this.x, this.y, this.width, this.height);
        };
      }
      //  control the animation of the platforms (text)
      function Platform(x, y, width, height, text) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.text = text;

        this.draw = () => {
          ctx.font = '16px Cinzel';
          ctx.fillStyle = 'forestgreen';
          ctx.fillText(this.text, this.x, this.y);
        };
      }

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
        powerUpArray.push(new PowerUp((canvas.width * 2 / 3) + 10, 140, 8, 20, 20, 1));
        platformArray.push(new Platform(100, 100, 188, 14, 'I\'ve build video games'));
        platformArray.push(new Platform((canvas.width * 1 / 2), 180, 102, 14, 'using Unity'));
        platformArray.push(new Platform(30, 230, 102, 14, 'coded in C#'));
        platformArray.push(new Platform((canvas.width / 4), 320, 233, 14, 'I\'ve also  used HTML canvas'));
        platformArray.push(new Platform(10, 380, 207, 14, 'and coded in javascript'));
        platformArray.push(new Platform(canvas.width - 180, 420, 163, 14, 'give this one a try!'));
        makeBase();
        makeBaseOne();
        makeBaseTwo();
        makeBaseThree();
        makeBaseOneA();
        makeBaseTwoA();
        makeBaseThreeA();
        makePowerUps();
        ctx.fillText(`Skills: ${skills}`, canvas.width - 150, 15);
      }

      function getDistance(x1, y1, x2, y2) {
        const xDistance = x2 - x1;
        const yDistance = y2 - y1;

        return Math.sqrt((xDistance ** 2) + (yDistance ** 2));
      }

      function WaitForJump() {
        setTimeout(() => {
          waitToTest = false;
        }, 200);
      }

      //  start of touch screen controls
      let clientX;
      let clientY;
      canvas.addEventListener('touchstart', (e) => {
        e.preventDefault();
        clientX = e.touches[0].clientX;
        clientY = e.touches[0].clientY;
        if (clientX > ball.x) {
          ball.dx = 3;
        } else if (clientX < ball.x) {
          ball.dx = -3;
        }
      }, false);

      canvas.addEventListener('touchend', (e) => {
        const deltaX = e.changedTouches[0].clientX - clientX;
        const deltaY = e.changedTouches[0].clientY - clientY;
        if (deltaY < -10) {
          grounded = false;
          platform = false;
          ball.jump();
          waitToTest = true;
          WaitForJump();
        }
        if (deltaX > 0) {
          ball.dx = 3;
          setTimeout(() => {
            ball.dx = 0;
          }, 300);
        }
        if (deltaX < 0) {
          ball.dx = -3;
          setTimeout(() => {
            ball.dx = 0;
          }, 300);
        }
      }, false);
      //  end of touch screen controles start of keyboard controls
      //  (these are unlikely to be useful on smaller devices, it still
      //  may be possible to attach a keyboard or something, so I left them
      window.addEventListener('keydown', (event) => {
        if (event.key === 'ArrowRight' || event.key === 'd') {
          ball.dx = 3;
        } else if (event.key === 'ArrowLeft' || event.key === 'a') {
          ball.dx = -3;
        } else if ((event.key === ' ' && grounded) || (event.key === 'w' && grounded)) {
          event.preventDefault();
          grounded = false;
          platform = false;
          ball.jump();
          waitToTest = true;
          WaitForJump();
        } else if (event.key === ' ' && !grounded) {
          event.preventDefault();
        } else if (event.key === 'w' && !grounded) {
          event.preventDefault();
        }
      });
      window.addEventListener('keyup', (event) => {
        if (event.key === 'ArrowRight' || event.key === 'd') {
          ball.dx = 0;
        } else if (event.key === 'ArrowLeft' || event.key === 'a') {
          ball.dx = 0;
        }
      });
      //  end of keyboard controls
      //  control behavior during a screen resize event
      window.addEventListener('resize', () => {
        canvas.width = window.innerWidth;
        init();
      });

      //  determine if the player is touching something that is considered ground.
      function groundCheck() {
        platform = false;
        base = getDistance(ball.x, (ball.y + (ball.height / 2)), ball.x, canvas.height);
        for (let i = 0; i < platformArray.length; i += 1) {
          if (platformArray[i].x < ball.x && ball.x < (platformArray[i].x + platformArray[i].width)
            && (ball.y + ball.height / 2) - platformArray[i].y <= 3
            && getDistance(ball.x, (ball.y + ball.height / 2), ball.x, platformArray[i].y)
            < platformArray[i].height * 2.2 && !waitToTest) {
            grounded = true;
            platform = true;
            ball.dy = 0;
            ball.y = platformArray[i].y - platformArray[i].height * 2.1;
          }
        }
        if (platform) {
          gravity = 0;
        } else if ((base <= 0.7 || (ball.y + (ball.height / 2)) > canvas.height) && !waitToTest) {
          grounded = true;
          //  the line below shouldn't be in this section
          ball.y = canvas.height - (ball.height / 2);
        } else {
          gravity = 1;
          grounded = false;
          platform = false;
        }
      }

      //  handle the power-up icons and check for victory conditions.

      function handlePowerUps() {
        for (let i = 0; i < powerUpArray.length; i += 1) {
          if (getDistance(ball.x - 4, ball.y, powerUpArray[i].x, powerUpArray[i].y)
            <= (ball.width / 2) + 8 && powerUpArray[i].visi >= 0.5) {
            powerUpArray[i].visi = 0;
            skills += 1;
          } else if (skills >= powerUpArray.length) {
            document.getElementById('cv').removeAttribute('class');
          }
        }
      }


      //  handel the animation of the canvas (and everything on it ultimately)
      function animate() {
        requestAnimationFrame(animate);
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        groundCheck();
        ball.update();
        for (let i = 0; i < powerUpArray.length; i += 1) {
          if (powerUpArray[i].visi >= 0.99) {
            powerUpArray[i].draw();
          }
        }
        handlePowerUps();
        for (let j = 0; j < platformArray.length; j += 1) {
          platformArray[j].draw();
        }
        ctx.font = '22px Times New Roman';
        ctx.fillStyle = 'black';
        ctx.fillText(`Skills:  ${skills}`, canvas.width - 150, 15);
      }

      init();
      animate();
    },
  },
  watch: {
    exampleContent() {
      this.updateCanvas();
      this.updateCanvasTwo();
    },
  },
  mounted() {
    this.updateCanvas();
    this.updateCanvasTwo();
  },
};
</script>

<style>
  .resumeHolder {
    display: none;
  }
  .resumeButton {
    position: absolute;
    font-size: 1.5em;
    bottom: 300px;
    left: 40%;
    width: 250px;
    height: 60px;
    background-color: seagreen;
    color: white;
    border: 2px solid rgba(219, 219, 219, 0.5);
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
