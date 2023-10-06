# Google Dino Game - Proj 1

![alt text](https://github.com/bryancyy192/bryancyy192.github.io/blob/main/images/screenshot.png)

### Try out the game here!

http://127.0.0.1:5500/index.html

Quick walkthrough on how i made the dino jump and duck!

I started off by setting the jump speed and gravity values as shown below.
```js
JUMP_SPEED = 0.6;
GRAVITY = 0.4;
```
Following this i added event listeners for keydown and keyup events and tagged them to the spacebar for jumping as well as the arrow down key to duck
```js
    window.removeEventListener("keydown", this.keydown);
    window.removeEventListener("keyup", this.keyup);
  
    window.addEventListener("keydown", this.keydown);
    window.addEventListener("keyup", this.keyup);

     keydown = (event) => {
      if (event.code === "Space") {
        this.jumpPressed = true;
      }
      else if (event.code === "ArrowDown"){
        this.duckPressed = true;
      }
    };
  
    keyup = (event) => {
      if (event.code === "Space") {
        this.jumpPressed = false;
      }
      else if(event.code === "ArrowDown"){
        this.duckPressed = false;
      }
    };
```

Added update to call upon the jump and duck functions when the respective keys are pressed.
```js
update(gameSpeed, frameTimeDelta) {
      this.run(gameSpeed, frameTimeDelta);
  
        if (this.jumpInProgress) {
            this.image = this.standingStillImage;
      }
        else if(this.duckInProgress){
            this.image = this.standingStillImage;
      }
  
        if(this.duckInProgress || this.duckPressed){
            this.duck(frameTimeDelta);
        }
        else if(this.jumpInProgress || this.jumpPressed){
            this.jump(frameTimeDelta);
        }
    }
```

Started off with the duck function as it is simpler to create.
```js
 duck(frameTimeDelta){
        if (this.duckPressed) {
            this.duckInProgress = true;
          }
          this.y = 10 + this.canvas.height - this.height - 1.5;
          this.duckInProgress = false;
}
```

Next I created the jump function step by step.

This portion ensures the player is still holding down the spacebar and has not reached the maxJumpHeight, the dinosaur will continue travelling up.
```js
 jump(frameTimeDelta) {
      if (this.jumpPressed) {
        this.jumpInProgress = true;
      }
  
      if (this.jumpInProgress && !this.falling) {
        if (
          this.y > this.canvas.height - this.minJumpHeight ||
          (this.y > this.canvas.height - this.maxJumpHeight && this.jumpPressed)
        ) {
          this.y -= this.JUMP_SPEED * frameTimeDelta * this.scaleRatio;
        } else {
          this.falling = true;
        }
      }
```

This next portion of the jump function reads when the player has released the spacebar or the dinosaur has reached the maxJumpHeight, causing gravity to take effect and the dinosaur to fall back to the ground.
```js
else {
        if (this.y < this.yStandingPosition) {
          this.y += this.GRAVITY * frameTimeDelta * this.scaleRatio;
          if (this.y + this.height > this.canvas.height) {
            this.y = this.yStandingPosition;
          }
        } else {
          this.falling = false;
          this.jumpInProgress = false;
        }
      }
    }
```

These are the main two factors which run the game! Allowing the player to jump and duck over obstacles that come in the way.

Thank you!