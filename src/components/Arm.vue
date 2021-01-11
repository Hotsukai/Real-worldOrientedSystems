<template>
  <div class="canvas">
    <form>
      <input
        v-model.number="thetas[index]"
        v-for="(theta, index) in thetas"
        :key="index + 't'"
        type="number"
      />度
    </form>
   
    x1:{{ p_start_end(0, 0)[0][0] }} b {{ p_start_end(0, 0)[1][0] }} c
    {{ p_start_end(0, 1)[0][0] }} d {{ p_start_end(0, 1)[1][0] }}
    <svg viewBox="0 0 1000 1000">
      <line
        :x1="p_start_end(0, 1)[0][0]"
        :y1="p_start_end(0, 1)[1][0]"
        :x2="p_start_end(0, 2)[0][0]"
        :y2="p_start_end(0, 2)[1][0]"
        stroke="#eaffd0"
        :stroke-width="20 + 'px'"
      ></line>
    </svg>
  </div>
</template>

<script>
export default {
  name: "Arm",
  data() {
    return {
      p: [
        [500, 500],
        [0, 100],
        [0, 100],
        [0, 100],
      ],
      thetas: [0, 0, 0],
    };
  },
  methods: {
    //@return [x,y]
    p_start_end(start, end) {
      let resultOfRxP;
      if (end - start > 1) {
        resultOfRxP = this.dot(
          this.rotateMatrix(this.thetas[start]),
          this.p_start_end(start + 1, end)
        );
      } else {
        resultOfRxP = this.dot(
          this.rotateMatrix(this.thetas[start]),
          this.transpose([this.p.slice(-1)[0]])
        );
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
      console.debug("matrixes : ", matrix1, matrix2);
      console.debug(
        "lengthes : ",
        rowCountMatrix1,
        rowCountMatrix2,
        colCountMatrix1,
        colCountMatrix2
      );
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
      console.debug("transpose before : " + JSON.stringify(a));
      console.debug(
        "transpose after : " +
          JSON.stringify(a[0].map((_, c) => a.map((r) => r[c])))
      );
      return a[0].map((_, c) => a.map((r) => r[c]));
    },
  },
  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.canvas {
  height: 1000px;
  width: 1000px;
}
</style>
