let ball;
let leftPaddle;
let rightPaddle;
let leftScore = 0;
let rightScore = 0;

function setup() {
  createCanvas(800, 400);
  ball = new Ball();
  leftPaddle = new Paddle(true);
  rightPaddle = new Paddle(false);
}

function draw() {
  background(0);
  
  ball.update();
  ball.checkPaddleCollision(leftPaddle);
  ball.checkPaddleCollision(rightPaddle);
  ball.checkBoundaryCollision();
  ball.display();
  
  leftPaddle.update();
  leftPaddle.checkBoundary();
  leftPaddle.display();
  
  rightPaddle.update();
  rightPaddle.checkBoundary();
  rightPaddle.display();
  
  displayScores();
}

function keyPressed() {
  if (key === 'w' || key === 'W') {
    leftPaddle.moveUp();
  } else if (key === 's' || key === 'S') {
    leftPaddle.moveDown();
  } else if (keyCode === UP_ARROW) {
    rightPaddle.moveUp();
  } else if (keyCode === DOWN_ARROW) {
    rightPaddle.moveDown();
  }
}

function keyReleased() {
  if ((key === 'w' || key === 'W') || (key === 's' || key === 'S')) {
    leftPaddle.stop();
  } else if (keyCode === UP_ARROW || keyCode === DOWN_ARROW) {
    rightPaddle.stop();
  }
}

function displayScores() {
  textSize(32);
  fill(255);
  textAlign(CENTER, TOP);
  text(leftScore, width / 4, 10);
  text(rightScore, 3 * width / 4, 10);
}

class Ball {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.diameter = 20;
    this.xSpeed = 5;
    this.ySpeed = 5;
  }
  
  update() {
    this.x += this.xSpeed;
    this.y += this.ySpeed;
  }
  
  display() {
    fill(255);
    ellipse(this.x, this.y, this.diameter);
  }
  
  checkBoundaryCollision() {
    if (this.y < 0 || this.y > height) {
      this.ySpeed *= -1;
    }
    
    if (this.x < 0) {
      rightScore++;
      this.reset();
    } else if (this.x > width) {
      leftScore++;
      this.reset();
    }
  }
  
  checkPaddleCollision(paddle) {
    if (this.x - this.diameter / 2 < paddle.x + paddle.width &&
        this.x + this.diameter / 2 > paddle.x &&
        this.y - this.diameter / 2 < paddle.y + paddle.height &&
        this.y + this.diameter / 2 > paddle.y) {
      this.xSpeed *= -1;
    }
  }
  
  reset() {
    this.x = width / 2;
    this.y = height / 2;
  }
}

class Paddle {
  constructor(isLeft) {
    this.width = 10;
    this.height = 80;
    this.y = height / 2 - this.height / 2;
    if (isLeft) {
      this.x = 0;
    } else {
      this.x = width - this.width;
    }
    this.speed = 10;
    this.isMovingUp = false;
    this.isMovingDown = false;
  }
  
  display() {
    fill(255);
    rect(this.x, this.y, this.width, this.height);
  }
  
  update() {
    if (this.isMovingUp) {
      this.y -= this.speed;
    } else if (this.isMovingDown) {
      this.y += this.speed;
    }
  }
  
  moveUp() {
    this.isMovingUp = true;
  }
  
  moveDown() {
    this.isMovingDown = true;
  }
  
  stop() {
    this.isMovingUp = false;
    this.isMovingDown = false;
  }
  
  checkBoundary() {
    this.y = constrain(this.y, 0, height - this.height);
  }
}
