<template>
  <div>
    <form>
      <div v-for="(theta, index) in thetas" :key="index + 't'">
        <label>第{{ index + 1 }}関節</label
        ><input v-model.number="thetas[index]" type="number" step="0.05" />rad
      </div>
    </form>
    <div class="canvas">
      <svg viewBox="-200 -200 1000 1000">
        <text x="0" y="0" font-family="Verdana" font-size="35">0,0</text>

        <line
          :x1="p_start_end(0, 0)[0][0]"
          :y1="p_start_end(0, 0)[0][0]"
          :x2="p_start_end(0, 1)[0][0]"
          :y2="p_start_end(0, 1)[1][0]"
          stroke="#ff0000"
          :stroke-width="20 + 'px'"
        ></line>
        <text
          :x="p_start_end(0, 1)[0][0]"
          :y="p_start_end(0, 1)[1][0]"
          font-family="Verdana"
          font-size="35"
        >
          {{ coordinateText(p_start_end(0, 1)) }}
        </text>
        <line
          :x1="p_start_end(0, 1)[0][0]"
          :y1="p_start_end(0, 1)[1][0]"
          :x2="p_start_end(0, 2)[0][0]"
          :y2="p_start_end(0, 2)[1][0]"
          stroke="#00ff00"
          :stroke-width="20 + 'px'"
        ></line>
        <text
          :x="p_start_end(0, 2)[0][0]"
          :y="p_start_end(0, 2)[1][0]"
          font-family="Verdana"
          font-size="35"
        >
          {{ coordinateText(p_start_end(0, 2)) }}
        </text>
        <line
          :x1="p_start_end(0, 2)[0][0]"
          :y1="p_start_end(0, 2)[1][0]"
          :x2="p_start_end(0, 3)[0][0]"
          :y2="p_start_end(0, 3)[1][0]"
          stroke="#0000ff"
          :stroke-width="20 + 'px'"
        ></line>
        <text
          :x="p_start_end(0, 3)[0][0]"
          :y="p_start_end(0, 3)[1][0]"
          font-family="Verdana"
          font-size="35"
        >
          {{ coordinateText(p_start_end(0, 3)) }}
        </text>
        <line
          :x1="p_start_end(0, 3)[0][0]"
          :y1="p_start_end(0, 3)[1][0]"
          :x2="p_start_end(0, 3)[0][0]"
          :y2="p_start_end(0, 3)[1][0]"
          stroke="#eaffd0"
          :stroke-width="20 + 'px'"
        ></line>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: "Arm",
  data() {
    return {
      p: [
        [0, 0],
        [0, 200],
        [0, 150],
        [0, 100],
      ],
      thetas: [-0.75, -1, 1],
    };
  },
  methods: {
    coordinateText(coordinateArray) {
      return `(x,y)=(${Math.round(coordinateArray[0][0] * 10) / 10},${
        Math.round(coordinateArray[1][0] * 10) / 10
      })`;
    },
    //@return [x,y]
    p_start_end(start, end) {
      let resultOfRxP;
      if (start > end) {
        console.error("引数が異常です");
      } else if (start === end) {
        return [[this.p[start][0]], [this.p[start][1]]];
      } else if (end - start > 1) {
        resultOfRxP = this.dot(
          this.rotateMatrix(this.thetas[start]),
          this.p_start_end(start + 1, end)
        );
      } else if (end - start === 1) {
        resultOfRxP = this.dot(
          this.rotateMatrix(this.thetas[start]),
          this.transpose([this.p[end]])
        );
      } else {
        console.error("エラー");
      }
      return [
        [this.p[start][0] + resultOfRxP[0][0]], //x軸
        [this.p[start][1] + resultOfRxP[1][0]], //y軸
      ];
      //
    },

    //@param [[...]] , [[],..,[]]
    dot(matrix1, matrix2) {
      const result = [];
      var rowCountMatrix1 = matrix1.length;
      var rowCountMatrix2 = matrix2.length;
      var colCountMatrix1 = matrix1[0].length;
      var colCountMatrix2 = matrix2[0].length;

      if (colCountMatrix1 !== rowCountMatrix2) {
        console.error("行列が不正です");
        return -1000;
      }
      for (var i1 = 0; i1 < rowCountMatrix1; i1++) {
        result.push([]);
        for (var i2 = 0; i2 < colCountMatrix2; i2++) {
          result[i1].push(0);
          for (var i3 = 0; i3 < colCountMatrix1; i3++) {
            result[i1][i2] += matrix1[i1][i3] * matrix2[i3][i2];
          }
        }
      }
      return result;
    },
    // 回転行列を返す
    rotateMatrix(theta) {
      return [
        [Math.cos(theta), -1 * Math.sin(theta)],
        [Math.sin(theta), Math.cos(theta)],
      ];
    },
    transpose(a) {
      return a[0].map((_, c) => a.map((r) => r[c]));
    },
  },
  computed: {},
};
</script>

<style scoped>
.canvas {
  height: 100vh;
  border: 1px solid #000;
}
</style>