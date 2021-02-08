<template>
  <v-container>
    <v-row>
      <v-col>
        <v-row>
          <TriangleInputForm @change="inputChanged" />
        </v-row>
        <v-row>
          <div>
            <p>{{ sideType }}</p>
            <p>{{ angleType }}</p>
          </div>
        </v-row>
      </v-col>
      <v-col>
        <TriangleCanvas
          :key="triangleCanvasKey"
          :side-a="sideA"
          :side-b="sideB"
          :side-c="sideC"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import TriangleCanvas from "./../components/TriangleCanvas";
import TriangleInputForm from "./../components/TriangleInputForm";
export default {
  name: "Home",

  components: {
    TriangleCanvas,
    TriangleInputForm,
  },
  data() {
    return {
      sideType: "",
      angleType: "",
      sideA: 0,
      sideB: 0,
      sideC: 0,
      triangleCanvasKey: "",
    };
  },
  methods: {
    inputChanged({ a, b, c }) {
      let isValidTriangle = this.validateTriangle(a, b, c);
      if (isValidTriangle) {
        this.getTriangleInfo(a, b, c);
        this.drawTriangle(a, b, c);
      }
    },
    validateTriangle(a, b, c) {
      if (a && b && c) {
        let list = [a, b, c];
        list.sort();
        if (list[2] > list[0] + list[1]) {
          return true;
        } else {
          return false;
        }
      } else {
        return false;
      }
    },
    getTriangleInfo(a, b, c) {
      // side info
      if (a !== b && b !== c) {
        this.sideType = "Scalene";
      } else if (this.isIsoceles(a, b, c)) {
        this.sideType = "Isoceles";
      } else if (a === b && b === c) {
        this.sideType = "Equilateral";
      }

      // degree info
      //   C = arccos ((a2 + b2 - c2) / 2ab)

      //   let C = Math.acos(
      //     ((parseInt(Math.pow(a, 2)) +
      //       parseInt(Math.pow(b, 2)) +
      //       parseInt(Math.pow(c, 2))) /
      //       2) *
      //       parseInt(a) *
      //       parseInt(b)
      //   );
      //   console.log(C);
    },
    isIsoceles(a, b, c) {
      if (a === b && a != c) {
        return true;
      } else if (a === c && a != b) {
        return true;
      } else {
        return false;
      }
    },
    drawTriangle(a, b, c) {
      this.sideA = parseInt(a);
      this.sideB = parseInt(b);
      this.sideC = parseInt(c);

      this.triangleCanvasKey = a + b + c;
    },
  },
};
</script>
