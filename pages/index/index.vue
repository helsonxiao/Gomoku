<template>
  <view>
    <canvas @touchend="handleTouchEnd" style="width: 100%; height: 500px;" canvas-id="myCanvas"></canvas>
  </view>
</template>

<script>
  export default {
    onReady: function() {
      this.rowCount = 7;
      this.columnCount = 7;
      this.padding = 50;
      this.radius = 10;

      this.boardTask = setTimeout(() => {
        this.ctx = uni.createCanvasContext('myCanvas');
        this.designBoard();
        this.ctx.draw();
      }, 100)


      this.isMe = true;
      this.chessBox = [];
      for (let i = 0; i < this.rowCount; i += 1) {
        this.chessBox[i] = []
        for (let j = 0; j < this.columnCount; j += 1) {
          this.chessBox[i][j] = 0;
        }
      }
    },
    onUnload() {
      clearTimeout(this.boardTask);
    },
    methods: {
      designBoard() {
        for (let i = 1; i <= this.rowCount; i += 1) {
          this.ctx.moveTo(0, this.padding * i);
          this.ctx.lineTo(400, this.padding * i);
        }
        for (let j = 1; j <= this.columnCount; j += 1) {
          this.ctx.moveTo(this.padding * j, 0);
          this.ctx.lineTo(this.padding * j, 400);
        }
        this.ctx.stroke();
      },
      designChess(x, y) {
        if (this.chessBox[x][y] !== 0) {
          return;
        }

        this.chessBox[x][y] = this.isMe ? 1 : 2;
        this.isMe = !this.isMe;
        this.ctx.beginPath();
        this.ctx.arc(
          (x + 1) * this.padding,
          (y + 1) * this.padding,
          this.radius,
          0,
          2 * Math.PI,
        );
        this.ctx.closePath();
        this.ctx.fillStyle = this.chessBox[x][y] === 1 ? 'black' : 'red';
        this.ctx.fill();
      },
      handleTouchEnd(event) {
        const pos = event.changedTouches[0];
        const x = Math.floor((pos.x - 2 * this.radius) / this.padding);
        const y = Math.floor((pos.y - 2 * this.radius) / this.padding);
        this.designChess(x, y);
        // this.designBoard();
        this.ctx.draw(true);
      },
      // writeMessage(message) {
      //   this.context.clearRect(0, 0, this.context.width, this.context.height);
      //   this.context.font = 'italic 20px "Fira Sans", serif';
      //   this.context.fillText(message, 25, 25);
      //   this.context.draw();
      // },
    }
  }
</script>

<style>

</style>
