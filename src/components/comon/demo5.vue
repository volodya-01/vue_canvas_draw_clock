<template>
  <div class="coolClockWrap" ref="coolClockWrapbox">
    <canvas ref="coolClockCanvas" :width="canvas.width" :height="canvas.height" class="cvas1"></canvas>
      <canvas ref="coolClockCanvas2" :width="canvas.width" :height="canvas.height" class="cvas2"></canvas>
  </div>
</template>

<script>
var elementResizeDetectorMaker = require("element-resize-detector");
export default {
  name: "demo5",
  data() {
    return {
      ctx: null,
      CFG: {
        perDeg: 1
      },
      deg: 0,
      circledata: {}, //基础圆参数
      canvas: {
        width: 630,
        height: 830
      },
      R: 0 //基础半径
    };
  },
  mounted() {
    this.resizefun();
  },
  destroyed() {
    window.cancelAnimationFrame(this.loop);
    window.cancelAnimationFrame(this.loop2);
  },
  methods: {
    //自适应方法
    resizefun() {
      var erd = elementResizeDetectorMaker();
      erd.listenTo(document.body, element => {
        var width = element.clientWidth * 0.35;
        var height = element.clientHeight * 0.75;
        this.canvas = { width, height };
        this.$nextTick(() => {
          console.log("Size: " + this.canvas.width + "px" + this.canvas.height);
          //使canvas尺寸重置
          this.drawCoolClock();
          this.drawCoolClock2();
          console.log(
            "canvas尺寸: " +
              this.$refs.coolClockCanvas.width +
              "px" +
              this.$refs.coolClockCanvas.height
          );
        });
      });
    },
    drawCoolClock() {
      let canvas = this.$refs.coolClockCanvas;
      let ctx = canvas.getContext("2d");
      let [x0, y0] = [this.canvas.width / 2, this.canvas.height / 2]; //获取圆心x,y
      this.R = this.canvas.width / 2.5; //设置圆半径
      this.circledata = { ctx, x0, y0 }; //第0圈(扫描帧动画初始化参数)
      this.init(); //第0圈(扫描帧动画初始化)
      ctx.clearRect(0, 0, canvas.width, canvas.height); //清除画布
    },
    /* 扫描开始 */
    init() {
      this.circledata.ctx.save();
      this.circledata.ctx.strokeStyle = "rgba(0,255,0,1)";
      this.circledata.ctx.fillStyle = "rgba(0,50,0,1)";
      this.circledata.ctx.arc(
        this.circledata.x0,
        this.circledata.y0,
        this.circledata.R,
        0,
        2 * Math.PI
      );
      this.circledata.ctx.fill();
      window.requestAnimationFrame(this.loop);
    },
    loop() {
      this.deg = this.deg + this.CFG.perDeg;
      this.drawcover(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R,
        this.deg
      );
      this.drawScale1(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第1圈(描边，填充)
      this.drawScale2(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第2圈(实际数据)

      this.drawScale3(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第3圈（实际数据）
      this.drawScale4(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第4圈(描边，填充)
      this.drawScale5(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第5圈(描边，填充)
      this.drawScale6(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第6圈（实际数据)
      // this.drawScale7(
      //   this.circledata.ctx,
      //   this.circledata.x0,
      //   this.circledata.y0,
      //   this.R
      // ); //绘制表盘第7圈（实际汇总数据)
      // this.drawScale8(
      //   this.circledata.ctx,
      //   this.circledata.x0,
      //   this.circledata.y0,
      //   this.R
      // ); //绘制表盘第8圈(描边，填充)
      // this.drawScale9(
      //   this.circledata.ctx,
      //   this.circledata.x0,
      //   this.circledata.y0,
      //   this.R
      // ); //绘制表盘第9圈(画一段渐变的圆弧)
      // this.drawScale10(
      //   this.circledata.ctx,
      //   this.circledata.x0,
      //   this.circledata.y0,
      //   this.R
      // ); //绘制表盘第10圈(加文字)
      this.drawSM(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R,
        this.deg
      );
      window.requestAnimationFrame(this.loop);
    },

    drawcover(ctx, x0, y0, R, iDeg) {
      // ctx.save();
      // ctx.beginPath();
      // ctx.fillStyle = "rgba(41,41,52,0.02)"
      // ctx.closePath();
      // ctx.fill();
      // ctx.restore();
      ctx.save();
      ctx.fillStyle = "rgba(41,41,52,1)";
      ctx.beginPath();
      ctx.moveTo(x0, y0);
      ctx.arc(
        x0,
        y0,
        R,
        ((-22 * this.CFG.perDeg + iDeg) / 90) * Math.PI,
        ((0 + iDeg) / 90) * Math.PI
      );
      ctx.closePath();
      ctx.fill();

      ctx.restore();
    },
    //第0圈(扫描)
    drawSM(ctx, x0, y0, R, iDeg) {
      ctx.save();
      ctx.fillStyle = "rgba(255,255,255,0.02)";
      ctx.beginPath();
      ctx.moveTo(x0, y0);
      ctx.arc(
        x0,
        y0,
        R,
        ((-22 * this.CFG.perDeg + iDeg) / 90) * Math.PI,
        ((0 + iDeg) / 90) * Math.PI
      );
      ctx.closePath();
      ctx.fill();

      ctx.restore();
    },

    /* 扫描结束 */
    //第1圈(描边，填充)
    drawScale1(ctx, x0, y0, R) {
      ctx.save();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "rgba(52,63,74,.7)";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // 绘画圆环
      ctx.beginPath();
      ctx.arc(x0, y0, R, 0, Math.PI * 2, true);

      ctx.stroke();
      ctx.closePath();
      ctx.fillStyle = "rgba(41,41,52,1)"; //填充圆
      ctx.fill();
      ctx.restore();
    },
    //第2圈(实际数据)
    drawScale2(ctx, x0, y0, R) {
      for (let i = 0; i < 720; i++) {
        let angel = -90 + i * (360 / 720); //角度
        let [x1, y1] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.9,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.9
        ];
        let [x2, y2] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.85,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.85
        ];
        //
        ctx.strokeStyle = "#245274";

        //
        ctx.save();
        ctx.beginPath();
        ctx.lineWidth = 1;
        ctx.lineCap = "round";
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.stroke();
        ctx.closePath();
        ctx.restore();
      }
    },

    //第3圈(实际数据)
    drawScale3(ctx, x0, y0, R) {
      for (let i = 0; i < 720; i++) {
        let angel = -90 + i * (360 / 720); //角度
        let [x1, y1] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.83,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.83
        ];
        let [x2, y2] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.72,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.72
        ];
        //
        ctx.strokeStyle = "#245274";

        //
        ctx.save();
        ctx.beginPath();
        ctx.lineWidth = 1;
        ctx.lineCap = "round";
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.stroke();
        ctx.closePath();
        ctx.restore();
      }
    },
    //第4圈(描边，填充)
    drawScale4(ctx, x0, y0, R) {
      ctx.save();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // 绘画圆环
      ctx.beginPath();
      ctx.arc(x0, y0, R * 0.69, 0, Math.PI * 2, true);
      ctx.stroke();
      ctx.closePath();
      ctx.fillStyle = "rgba(41,41,52,0)"; //填充圆
      ctx.fill();
      ctx.restore();
    },
    //第5圈(描边，填充)
    drawScale5(ctx, x0, y0, R) {
      ctx.save();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // 绘画圆环
      ctx.beginPath();
      ctx.arc(x0, y0, R * 0.58, 0, Math.PI * 2, true);
      ctx.stroke();
      ctx.closePath();
      //ctx.fillStyle = "#292934"; //填充圆
      //ctx.fill();
      ctx.restore();
    },
    //第6圈(实际数据)
    drawScale6(ctx, x0, y0, R) {
      for (let i = 0; i < 720; i++) {
        let angel = -90 + i * (360 / 720); //角度
        let [x1, y1] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.56,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.56
        ];
        let [x2, y2] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.5,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.5
        ];
        //
        ctx.strokeStyle = "#245274";

        //
        ctx.save();
        ctx.beginPath();
        ctx.lineWidth = 1;
        ctx.lineCap = "round";
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.stroke();
        ctx.closePath();
        ctx.restore();
      }
    },
    //第7圈（实际汇总数据)
    drawScale7(ctx, x0, y0, R) {
      for (let i = 0; i < 720; i++) {
        let angel = -90 + i * (360 / 720); //角度
        let [x1, y1] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.48,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.48
        ];
        let [x2, y2] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.35,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.35
        ];
        //
        ctx.strokeStyle = "#292934";
        if (i > 0 && i < 99) {
          ctx.strokeStyle = "#836B27";
        } else if (i > 103 && i < 359) {
          ctx.strokeStyle = "#4CC2CE";
        } else if (i > 369 && i < 712) {
          ctx.strokeStyle = "rgb(108,40,46)";
        } else {
          ctx.strokeStyle = "#292934";
        }
        //
        ctx.save();
        ctx.beginPath();
        ctx.lineWidth = 3;
        ctx.lineCap = "round";
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.stroke();
        ctx.closePath();
        ctx.restore();
      }
    },
    //第8圈(描边,填充)
    drawScale8(ctx, x0, y0, R) {
      ctx.save();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // 绘画圆环
      ctx.beginPath();
      ctx.arc(x0, y0, R * 0.31, 0, Math.PI * 2, true);
      ctx.stroke();
      ctx.closePath();
      //ctx.fillStyle = "#292934"; //填充圆
      //ctx.fill();
      ctx.restore();
    },
    //第9圈(描边,填充)
    drawScale9(ctx, x0, y0, R) {
      ctx.save();
      let ring = ctx.createLinearGradient(0, 60, 60, 0);
      // 通过修改中间的值可以改变圆中颜色的饱和度:圆环
      // createLinearGradient(x0,y0,x1,y1)；
      // 参数分别是渐变开始的x0、y0坐标；渐变结束点的x1、y1坐标
      // var ring = ctx.createLinearGradient(0, 60, 60, 0);
      ring.addColorStop("0", "rgba(255,255,255,1)");
      ring.addColorStop("0.1", "rgba(255,255,255,.4)");
      ring.addColorStop("0.2", "rgba(255,255,255,.3)");
      ring.addColorStop("0.4", "rgba(255,255,255,.25)");
      ring.addColorStop("0.6", "rgba(255,255,255,.2)");
      ring.addColorStop("0.8", "rgba(255,255,255,.15)");
      ring.addColorStop("1.0", "rgba(255,255,255,0)");
      // 用渐变进行填充颜色
      ctx.strokeStyle = ring;
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // 绘画圆环
      ctx.beginPath();
      ctx.arc(x0, y0, R * 0.25, 0, Math.PI, false);
      ctx.stroke();
      ctx.closePath();
      ctx.restore();
    },
    //第10圈(加文字)
    drawScale10(ctx, x0, y0, R) {
      ctx.save();
      ctx.beginPath();
      ctx.font = `${R * 0.19}px MicrosoftYaHei`;
      ctx.fillStyle = "#B4B4B4"; //填充圆
      ctx.fill();
      ctx.fillText("24", x0 * 0.91, y0 * 0.98);
      ctx.closePath();
      ctx.beginPath();
      ctx.font = `${R * 0.078}px MicrosoftYaHei`;
      ctx.fillStyle = "#6C6C6C"; //填充圆
      ctx.fill();
      ctx.fillText("报警总数", x0 * 0.88, y0 * 1.08);
      ctx.closePath();
      ctx.restore();
    },
    /* 第二个canvas开始 */
    drawCoolClock2() {
      let canvas = this.$refs.coolClockCanvas2;
      let ctx = canvas.getContext("2d");
      let [x0, y0] = [this.canvas.width / 2, this.canvas.height / 2]; //获取圆心x,y
      this.R = this.canvas.width / 2.5; //设置圆半径
      this.circledata = { ctx, x0, y0 }; //第0圈(扫描帧动画初始化参数)
      this.init2(); //第0圈(扫描帧动画初始化)
      ctx.clearRect(0, 0, canvas.width, canvas.height); //清除画布
    },
    /* 扫描开始 */
    init2() {
      // this.circledata.ctx.save();
      // this.circledata.ctx.strokeStyle = "rgba(0,255,0,1)";
      // this.circledata.ctx.fillStyle = "rgba(0,50,0,1)";
      // this.circledata.ctx.arc(
      //   this.circledata.x0,
      //   this.circledata.y0,
      //   this.circledata.R,
      //   0,
      //   2 * Math.PI
      // );
      // this.circledata.ctx.fill();
      window.requestAnimationFrame(this.loop2);
    },
    loop2() {
      // this.deg = this.deg + this.CFG.perDeg;
      // this.drawcover(
      //   this.circledata.ctx,
      //   this.circledata.x0,
      //   this.circledata.y0,
      //   this.R,
      //   this.deg
      // );
      this.drawScale72(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第7圈（实际汇总数据)
      this.drawScale82(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第8圈(描边，填充)
      this.drawScale92(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第9圈(画一段渐变的圆弧)
      this.drawScale102(
        this.circledata.ctx,
        this.circledata.x0,
        this.circledata.y0,
        this.R
      ); //绘制表盘第10圈(加文字)
      // this.drawSM(
      //   this.circledata.ctx,
      //   this.circledata.x0,
      //   this.circledata.y0,
      //   this.R,
      //   this.deg
      // );
      window.requestAnimationFrame(this.loop2);
    },

    drawcover2(ctx, x0, y0, R, iDeg) {
      // ctx.save();
      // ctx.beginPath();
      // ctx.fillStyle = "rgba(41,41,52,0.02)"
      // ctx.closePath();
      // ctx.fill();
      // ctx.restore();
      ctx.save();
      ctx.fillStyle = "rgba(41,41,52,1)";
      ctx.beginPath();
      ctx.moveTo(x0, y0);
      ctx.arc(
        x0,
        y0,
        R,
        ((-22 * this.CFG.perDeg + iDeg) / 90) * Math.PI,
        ((0 + iDeg) / 90) * Math.PI
      );
      ctx.closePath();
      ctx.fill();

      ctx.restore();
    },
    //第0圈(扫描)
    drawSM2(ctx, x0, y0, R, iDeg) {
      ctx.save();
      ctx.fillStyle = "rgba(52,63,74,.7)";
      ctx.beginPath();
      ctx.moveTo(x0, y0);
      ctx.arc(
        x0,
        y0,
        R,
        ((-22 * this.CFG.perDeg + iDeg) / 90) * Math.PI,
        ((0 + iDeg) / 90) * Math.PI
      );
      ctx.closePath();
      ctx.fill();

      ctx.restore();
    },

    /* 扫描结束 */

    //第6圈(实际数据)
    drawScale62(ctx, x0, y0, R) {
      for (let i = 0; i < 720; i++) {
        let angel = -90 + i * (360 / 720); //角度
        let [x1, y1] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.56,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.56
        ];
        let [x2, y2] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.5,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.5
        ];
        //
        ctx.strokeStyle = "#245274";

        //
        ctx.save();
        ctx.beginPath();
        ctx.lineWidth = 1;
        ctx.lineCap = "round";
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.stroke();
        ctx.closePath();
        ctx.restore();
      }
    },
    //第7圈（实际汇总数据)
    drawScale72(ctx, x0, y0, R) {
      for (let i = 0; i < 720; i++) {
        let angel = -90 + i * (360 / 720); //角度
        let [x1, y1] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.48,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.48
        ];
        let [x2, y2] = [
          x0 + Math.cos((angel * Math.PI) / 180) * R * 0.35,
          y0 + Math.sin((angel * Math.PI) / 180) * R * 0.35
        ];
        //
        ctx.strokeStyle = "#292934";
        if (i > 0 && i < 99) {
          ctx.strokeStyle = "#836B27";
        } else if (i > 103 && i < 359) {
          ctx.strokeStyle = "#4CC2CE";
        } else if (i > 369 && i < 712) {
          ctx.strokeStyle = "rgb(108,40,46)";
        } else {
          ctx.strokeStyle = "#292934";
        }
        //
        ctx.save();
        ctx.beginPath();
        ctx.lineWidth = 3;
        ctx.lineCap = "round";
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.stroke();
        ctx.closePath();
        ctx.restore();
      }
    },
    //第8圈(描边,填充)
    drawScale82(ctx, x0, y0, R) {
      ctx.save();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // 绘画圆环
      ctx.beginPath();
      ctx.arc(x0, y0, R * 0.31, 0, Math.PI * 2, true);
      ctx.stroke();
      ctx.closePath();
      //ctx.fillStyle = "#292934"; //填充圆
      //ctx.fill();
      ctx.restore();
    },
    //第9圈(描边,填充)
    drawScale92(ctx, x0, y0, R) {
      ctx.save();
      let ring = ctx.createLinearGradient(0, 60, 60, 0);
      // 通过修改中间的值可以改变圆中颜色的饱和度:圆环
      // createLinearGradient(x0,y0,x1,y1)；
      // 参数分别是渐变开始的x0、y0坐标；渐变结束点的x1、y1坐标
      // var ring = ctx.createLinearGradient(0, 60, 60, 0);
      ring.addColorStop("0", "rgba(255,255,255,1)");
      ring.addColorStop("0.1", "rgba(255,255,255,.4)");
      ring.addColorStop("0.2", "rgba(255,255,255,.3)");
      ring.addColorStop("0.4", "rgba(255,255,255,.25)");
      ring.addColorStop("0.6", "rgba(255,255,255,.2)");
      ring.addColorStop("0.8", "rgba(255,255,255,.15)");
      ring.addColorStop("1.0", "rgba(255,255,255,0)");
      // 用渐变进行填充颜色
      ctx.strokeStyle = ring;
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // 绘画圆环
      ctx.beginPath();
      ctx.arc(x0, y0, R * 0.25, 0, Math.PI, false);
      ctx.stroke();
      ctx.closePath();
      ctx.restore();
    },
    //第10圈(加文字)
    drawScale102(ctx, x0, y0, R) {
      ctx.save();
      ctx.beginPath();
      ctx.font = `${R * 0.19}px MicrosoftYaHei`;
      ctx.fillStyle = "#B4B4B4"; //填充圆
      ctx.fill();
      ctx.fillText("24", x0 * 0.91, y0 * 0.98);
      ctx.closePath();
      ctx.beginPath();
      ctx.font = `${R * 0.078}px MicrosoftYaHei`;
      ctx.fillStyle = "#6C6C6C"; //填充圆
      ctx.fill();
      ctx.fillText("报警总数", x0 * 0.88, y0 * 1.08);
      ctx.closePath();
      ctx.restore();
    }
  }
};
</script>
<style lang="scss" scoped>
@import "@/styles/global.scss";
.coolClockWrap {
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
  align-items: center;
  width: vw(1030);
  height: vh(1630);
  background: #1b1b1b;
  position: relative;
  .cvas1 {
    position: absolute;
  }
  .cvas2 {
    position: absolute;
    z-index: 999;
  }
}
</style>