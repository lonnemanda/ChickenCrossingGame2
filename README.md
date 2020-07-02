# ChickenCrossingGame2

<!DOCTYPE html>
<html>
<head>
  <title>Crossing Game</title>
  <style type ="text/css">
    canvas{
      border: 2px solid black;
      background-color: red;
    }
    </style>
  </head>
  <body>
    <h1>Why did the Chicken Cross the Road</h1>
      <canvas id='myCanvas' width='1000' height='500'></canvas>
      <script type="text/javascript">
        var canvas = document.getElementById('myCanvas');
        var ctx = canvas.getContext ('2d');
        var isGameLive = true;

        let screenWidth = 1000;
        let screenHeight = 500;
        let width = 50;

        class GameCharacter{
          constructor(x, y, width, height, speed, sprite){
            this.x = x;
            this.y = y;
            this.width = width;
            this.height = height;
            this.speed = speed;
            this.maxSpeed = 4;
            this.sprite = sprite;
          }
          moveVertical() {
              if (this.y > screenHeight) //- this.height ||
                //this.y < 0) {
                {this.y = 0;
              }
              this.y += this.speed;
          }
          moveHorizontal(){
              this.x += this.speed;
          }
        }
        var enemies = [
          new GameCharacter(225, 50, 50, 100, 3,
            'images/fireTruck.png'),
          new GameCharacter(550, 300, 50, 150, 2,
            'images/semiTruck.png'),
          new GameCharacter(700, 200, 50, 50, 6,
            'images/sportsCar.png'),
        ];

        var player = new GameCharacter(10, 225, width, width,
          0, 'images/chicken.png')
        var goal = new GameCharacter(950, 200, 50, 100, 0,
           'images/chick.png')
        var sprites = {};

        var loadSprites = function() {
          sprites.player = new Image();
          sprites.player.src = player.sprite;

          sprites.goal = new Image();
          sprites.goal.src = goal.sprite;

          sprites.background = new Image();
          sprites.background.src ='images/background.png';

          enemies.forEach(function(element) {
              sprites.element = new Image();
            });
          enemies.forEach(function(element){
            sprites.element.src = element.sprite;
          });
        }

          document.onkeydown = function(event) {
            let keyPressed = event.keyCode;
            if (keyPressed == 68) {
              player.speed = player.maxSpeed;
            } else if (keyPressed == 65) {
              player.speed = -player.maxSpeed;
            }
          };
          document.onkeyup = function(event) {
            player.speed = 0;
          };

          var checkCollisions = function(rect1, rect2){

              let rect1x2 = rect1.x + rect1.width;
              let rect2x2 = rect2.x + rect2.width;
              let rect1y2 = rect1.y + rect1.height;
              let rect2y2 = rect2.y + rect2.height;

          return rect1.x < rect2x2 && rect1x2 >
          rect2.x && rect1.y < rect2y2 && rect1y2 > rect2.y;
          }
        var winCondition = function(rect1){
            let rect1x2 = rect1.x + rect1.width
          return rect1x2 > goal.x + 2
        }
        var draw = function() {
          ctx.clearRect(0, 0, screenWidth, screenHeight);

          ctx.drawImage (sprites.background, 0, 0);
          ctx.drawImage (sprites.player, player.x, player.y);
          ctx.drawImage (sprites.goal, goal.x, goal.y);

          //ctx.fillStyle = goal.color;
          //ctx.fillRect(goal.x,goal.y, goal.width, goal.height);
          //ctx.drawImage (sprites.enemies[0], enemies[0].x, 0);
          //ctx.drawImage (sprites.player, enemies[1].x, player.y);
          //ctx.drawImage (sprites.background, 0, 0);

          enemies.forEach(function(element) {
            ctx.drawImage(sprites.element, element.x, element.y)
          });
        //    ctx.fillStyle = element.color;
        //    ctx.fillRect(element.x, element.y,
          //    element.width,element.height);
        //  });
          //ctx.fillStyle = player.color;
          //ctx.fillRect(player.x, player.y,  player.width, player.height);
        }

        var update = function(){
          enemies.forEach(function(element) {
            if (checkCollisions(player, element)) {
              endGameLogic("Game Over!");
            }
            element.moveVertical();
          });
          player.moveHorizontal();

          if (winCondition(player)){
            endGameLogic("You Win!!!");
          }
        }

        var endGameLogic = function(text){
          isGameLive = false;
          alert(text);
          location.reload();
        }

        var step = function() {
          update();
          draw();
          if (isGameLive) {
            window.requestAnimationFrame(step);
          }
        }
        loadSprites();
        step();
      </script>
</body>
