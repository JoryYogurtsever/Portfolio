<template>
  <div>
    <canvas id="canvas">
    </canvas>
    <form action="" class="contact">
      <textarea name="Contact Us" id="" cols="30" rows="10" class=""></textarea>
      <button class="">poopy time!</button>
    </form>
  </div>
</template>

<script>
  export default {
    methods: {
      updateCanvas: function () {
        var canvas = document.getElementById('canvas'),
          ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = 600;
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = "black";

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
        window.addEventListener('resize',
        function() {
          canvas.width = window.innerWidth;

          init();
        });

        function Circle(x, y, dx, dy, radius, colorS) {
          this.x = x;
          this.y = y;
          this.dx = dx;
          this.dy = dy;
          this.radius = radius;
          this.startRadius = radius;
          this.colorS = colorS;

          this.draw = function () {
            ctx.beginPath();
            ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
            ctx.strokeStyle = "white";
            ctx.stroke();
            var grad = ctx.createRadialGradient(this.x, this.y, 0, this.x, this.y, this.radius);
            grad.addColorStop(0, colorArray[this.colorS]);
            grad.addColorStop(1, "palegreen");
            ctx.fillStyle = grad;
            ctx.fill();
          };

          this.update = function () {
            if (this.y + this.radius > innerHeight || this.y - this.radius < 0) {
              this.dy = -this.dy;
            }

            if (this.x + this.radius > innerWidth || this.x - this.radius < 0) {
              this.dx = -this.dx;
            }
            this.x += this.dx;
            this.y += this.dy;

            //interactivity
            if (mouse.x - this.x < 60 && mouse.x - this.x > -60
              && mouse.y - this.y < 60 && mouse.y - this.y > -60) {
              if (this.radius < 80) {
                this.radius += 1;
              }
            }
            else if (this.radius > this.startRadius) {
                this.radius -= 1;
              }
              this.draw();
            }
          };



            // let circle = new Circle(200, 200, 4, 4, 30);
        var circleArray = [];


          function init() {
            circleArray = [];
            for (var i = 0; i < 200; i++) {
              var colorS = Math.floor(Math.random() * colorArray.length);
              var radius = Math.random() * 30 + 1;
              var x = Math.random() * (innerWidth - radius * 2) + radius;
              var y = Math.random() * (innerHeight - radius * 2) + radius;
              var dy = (Math.random() - 0.5) * 2;
              var dx = (Math.random() - 0.5) * 2;
              circleArray.push(new Circle(x, y, dx, dy, radius, colorS))
            }
          }

          function animate() {
            requestAnimationFrame(animate);
            ctx.clearRect(0, 0, innerWidth, innerHeight);
            for (var i = 0; i < circleArray.length; i++) {
              circleArray[i].update();
            }
            //circle.update();

            /*ctx.beginPath();
          ctx.arc(x, y, 30, 0, Math.PI * 2, false);
          ctx.strokeStyle = "blue";
          ctx.stroke();*/
            //console.log('BOOBIES!');

            /*if (y + radius > innerHeight || y - radius < 0) {
            dy = -dy;
          }

          if (x + radius > innerWidth || x - radius < 0) {
            dx = -dx;
          }
          x += dx;
          y += dy;
        }*/
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
  #canvas{
    height:600px;
    z-index: 1;
    position: relative;
  }
  .contact {
    z-index: 2;
    position: absolute;
    left: 45%;
    /*top: 10px;*/
  }
</style>
