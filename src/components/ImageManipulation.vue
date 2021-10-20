<template>
  <div>
    <br />
    <br />
    <br />
    <br />
    <br />
    <div class="container">
      <div class="row my-5">
        <div class="col-5">
          <img :src="basePath" id="image" class="image" alt="" />
        </div>

        <div class="col-2">
          <br />
          <br />
          <select name="goals" v-model="curGoal">
            <option
              v-for="option in options"
              :value="option.value"
              :key="option.value"
              >{{ option.text }}</option
            >
          </select>
          <button @click="changeImage()" class="mt-3 btn btn-outline-primary">
            Apply Changes
          </button>
        </div>

        <div class="col-5">
          <div class="transition">
            <canvas id="canvas"></canvas>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import cv from ''
export default {
  data() {
    return {
      curGoal: 1,
      options: [
        { text: 'Meta 1', value: 1 },
        { text: 'Meta 2', value: 2 },
        { text: 'Meta 3', value: 3 },
        { text: 'Meta 4', value: 4 },
        { text: 'Meta 5', value: 5 },
        { text: 'Meta 6', value: 6 },
      ],
      applyChanges: false,
      show: false,
    };
  },
  computed: {
    basePath() {
      return `imgs/Imagem${this.curGoal}.jpg`;
    },
  },
  methods: {
    changeImage() {
      if (this.show) {
        this.show = false;
      }

      this.show = true;
      switch (this.curGoal) {
        case 1:
          this.goal1();
          break;

        case 2:
          this.goal2();
          break;

        case 3:
          this.goal3();
          break;

        case 4:
          this.goal4();
          break;

        case 5:
          this.goal5();
          break;

        case 6:
          this.goal6();
          break;
      }

      setTimeout(() => (this.show = false), 5000);
    },

    goal1() {
      let canvas = document.getElementById('canvas');

      let ctx = canvas.getContext('2d');

      if (this.show) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
      }

      let srcImg = document.getElementById('image');

      ctx.drawImage(srcImg, 0, 0, ctx.canvas.width, ctx.canvas.height);

      let imgData = ctx.getImageData(0, 0, ctx.canvas.width, ctx.canvas.height);
      let pixels = imgData.data;
      for (let i = 0; i < pixels.length; i += 4) {
        let lightness = parseInt(
          (pixels[i] + pixels[i + 1] + pixels[i + 2]) / 3
        );

        pixels[i] = lightness;
        pixels[i + 1] = lightness;
        pixels[i + 2] = lightness;
      }
      ctx.putImageData(imgData, 0, 0);
    },
    goal2() {
      let canvas = document.getElementById('canvas');

      let ctx = canvas.getContext('2d');

      if (this.show) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
      }

      let srcImg = document.getElementById('image');

      ctx.drawImage(srcImg, 0, 0, ctx.canvas.width, ctx.canvas.height);

      let imgData = ctx.getImageData(0, 0, ctx.canvas.width, ctx.canvas.height);

      let pixels = imgData.data;

      const brightness = 0.8;

      for (let i = 0; i < pixels.length; i++) {
        pixels[i] *= brightness;
        pixels[i + 1] *= brightness;
        pixels[i + 2] *= brightness;
      }
      ctx.putImageData(imgData, 0, 0);
    },

    goal3() {
      const img = document.getElementById('image');
      const canvas = document.getElementById('canvas');
      const ctx = canvas.getContext('2d');
      let d = ctx.getImageData(0, 0, ctx.canvas.width, ctx.canvas.height);
      const threshold = 35;

      ctx.drawImage(img, 0, 0);

      for (var i = 0; i < d.length; i += 4) {
        var r = d.data[i];
        var g = d.data[i + 1];
        var b = d.data[i + 2];
        var v = 0.2126 * r + 0.7152 * g + 0.0722 * b >= threshold ? 255 : 0;
        d.data[i] = d.data[i + 1] = d.data[i + 2] = v;
      }

      ctx.drawImage(d, 0, 0);
      console.log('opa');

      //NOVO
      // const img = document.getElementById('image');
      // const canvas = document.getElementById('canvas');
      // const ctx = canvas.getContext('2d');

      // ctx.drawImage(img, 0, 0);

      // const imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
      // const pixelData = imgData.data;

      // const kernelX = [1, 0, -1, 2, 0, -2, 1, 0, -1];
      // const kernelY = [1, 2, 1, 0, 0, 0, -1, -2, -1];
      // const size = Math.round(Math.sqrt(kernelX.length));
      // const threshold = 30;
      // const width = img.width;
      // const height = img.height;
      // const output = ctx.createImageData(width, height);

      // // ctx.drawImage(img, 0, 0, img.width, img.height);

      // for (let x = 1; x < width - 1; x++) {
      //   for (let y = 1; y < height - 1; y++) {
      //     let gx = 0;
      //     let gy = 0;

      //     for (let xk = 0; xk < size; xk++) {
      //       for (let yk = 0; yk < size; yk++) {
      //         let xa = x + (xk - 1);
      //         let ya = x + (yk - 1);

      //         if (ya >= 0 && ya < height && xa >= 0 && xa < width) {
      //           let pixel = pixelData[(ya * height + xa) * 4];
      //           let vx = kernelX[yk * size + xk];
      //           let vy = kernelY[yk * size + xk];
      //           gx += pixel * vx;
      //           gy += pixel * vy;
      //         }
      //       }
      //     }

      //     let g = Math.sqrt(Math.pow(gx, 2) + Math.pow(gy, 2));
      //     let p = 0;

      //     if (g > threshold) {
      //       p = 255;
      //     }
      //     let offset = (y * width + x) * 4;
      //     output.data[offset] = p;
      //     output.data[offset + 1] = p;
      //     output.data[offset + 2] = p;
      //     output.data[offset + 3] = 255;
      //   }
      // }

      // var canvas2 = ctx.putImageData(output, 0, 0);

      // document.body.appendChild(canvas2);

      // return output;
      //ANTIGO
      // const img = document.getElementById('image');
      // const canvas = document.getElementById('canvas');

      // const ctx = canvas.getContext('2d');

      // if (this.show) {
      //   ctx.clearRect(0, 0, canvas.width, canvas.height);
      // }

      // const threshold = 35;

      // ctx.drawImage(img, 0, 0, img.width, img.height);

      // const dimensions = {
      //   width: ctx.canvas.width,
      //   height: ctx.canvas.height,
      // };

      // let imgData = ctx.getImageData(0, 0, ctx.canvas.width, ctx.canvas.height);

      // const plotPoint = function(x, y, ctx) {
      //   ctx.beginPath();
      //   ctx.arc(x, y, 0.5, 0, 2 * Math.PI, false);
      //   ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
      //   // ctx.filter = 'blur(30px)';
      //   ctx.fill();
      //   ctx.beginPath();
      // };

      // for (let y = 0; y < imgData.height; y++) {
      //   for (let x = 0; x < imgData.width; x++) {
      //     let index = (x + y * dimensions.width) * 4;
      //     let pixel = imgData.data[index + 3];

      //     let left = imgData.data[index - 4];
      //     let right = imgData.data[index + 1];
      //     let top = imgData.data[index - dimensions.width * 4];
      //     let bottom = imgData.data[index + dimensions.width * 4];

      //     if (pixel > left + threshold) {
      //       plotPoint(x, y, ctx);
      //     } else if (pixel < left - threshold) {
      //       plotPoint(x, y, ctx);
      //     } else if (pixel > right + threshold) {
      //       plotPoint(x, y, ctx);
      //     } else if (pixel < right - threshold) {
      //       plotPoint(x, y, ctx);
      //     } else if (pixel > top + threshold) {
      //       plotPoint(x, y, ctx);
      //     } else if (pixel < top - threshold) {
      //       plotPoint(x, y, ctx);
      //     } else if (pixel > bottom + threshold) {
      //       plotPoint(x, y, ctx);
      //     } else if (pixel < bottom - threshold) {
      //       plotPoint(x, y, ctx);
      //     }
      //   }
      // }
    },
    goal4() {
      let canvas = document.getElementById('canvas');

      let ctx = canvas.getContext('2d');

      if (this.show) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
      }

      let srcImg = document.getElementById('image');

      ctx.drawImage(srcImg, 0, 0, ctx.canvas.width, ctx.canvas.height);

      let imgData = ctx.getImageData(0, 0, ctx.canvas.width, ctx.canvas.height);

      let pixels = imgData.data;

      const contrast = -30 / 100 + 1;
      var intercept = 128 * (1 - contrast);
      for (var i = 0; i < pixels.length; i += 4) {
        pixels[i] = pixels[i] * contrast + intercept;
        pixels[i + 1] = pixels[i + 1] * contrast + intercept;
        pixels[i + 2] = pixels[i + 2] * contrast + intercept;
      }

      ctx.putImageData(imgData, 0, 0);
    },
    goal5() {},
    goal6() {
      let canvas = document.getElementById('canvas');

      let ctx = canvas.getContext('2d');

      if (this.show) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
      }

      let srcImg = document.getElementById('image');

      ctx.drawImage(srcImg, 0, 0, ctx.canvas.width, ctx.canvas.height);

      let imgData = ctx.getImageData(0, 0, ctx.canvas.width, ctx.canvas.height);

      let pixels = imgData.data;

      for (let i = 0; i < pixels.length; i += 4) {
        let r = pixels[i];
        let g = pixels[i + 1];
        let b = pixels[i + 2];

        var invertedRed = 255 - r;
        var invertedGreen = 255 - g;
        var invertedBlue = 255 - b;

        pixels[i] = invertedRed;
        pixels[i + 1] = invertedGreen;
        pixels[i + 2] = invertedBlue;
      }
      ctx.putImageData(imgData, 0, 0);
    },
  },
};
</script>

<style scoped></style>
