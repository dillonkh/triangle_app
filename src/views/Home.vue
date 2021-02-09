<template>
  <v-container>
    <v-row>
      <v-col>
        <v-row>
          <TriangleInputForm @change="inputChanged" />
        </v-row>
        <v-row>
          <div v-if="isValid">
            <p>Valid {{ sideType }} {{ angleType }} Triangle!</p>
            <p>
              Angles:
              {{ angleA.toFixed(0) }}° {{ angleB.toFixed(0) }}°
              {{ angleC.toFixed(0) }}°
            </p>
          </div>
          <div v-else>
            <p>Not a Valid Triangle</p>
          </div>
        </v-row>
      </v-col>
      <v-col>
        <TriangleCanvas
          :key="triangleCanvasKey"
          :sideA="sideA"
          :sideB="sideB"
          :sideC="sideC"
          :angleA="angleA"
          :angleB="angleB"
          :angleC="angleC"
          :show="isValid"
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
      angleA: 0,
      angleB: 0,
      angleC: 0,
      triangleCanvasKey: "",
      isValid: false,
    };
  },
  methods: {
    inputChanged({ a, b, c }) {
      let isValidTriangle = this.validateTriangle(
        parseInt(a),
        parseInt(b),
        parseInt(c)
      );
      if (isValidTriangle) {
        this.getTriangleInfo(a, b, c);
        this.drawTriangle(a, b, c);
        this.isValid = true;
      } else {
        this.isValid = false;
        this.sideType = "";
        this.angleType = "";

        this.angleA = 0;
        this.angleB = 0;
        this.angleC = 0;
      }
    },
    validateTriangle(a, b, c) {
      if (a && b && c) {
        if (a + b > c && a + c > b && b + c > a) {
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
      this.getSideInfo(a, b, c);

      // degree info
      this.getAngleInfo(parseFloat(a), parseFloat(b), parseFloat(c));
    },
    getSideInfo(a, b, c) {
      this.sideA = parseInt(a);
      this.sideB = parseInt(b);
      this.sideC = parseInt(c);

      if (a !== b && b !== c) {
        this.sideType = "Scalene";
      } else if (this.isIsoceles(a, b, c)) {
        this.sideType = "Isoceles";
      } else if (a == b && b == c) {
        this.sideType = "Equilateral";
      }
    },
    getAngleInfo(aLen, bLen, cLen) {
      // get Degree for C
      let sideList = [aLen, bLen, cLen];
      sideList.sort();

      let a = sideList[0];
      let b = sideList[1];
      let c = sideList[2];

      let cInRadians = Math.acos(
        (Math.pow(a, 2) + Math.pow(b, 2) - Math.pow(c, 2)) / (2 * a * b)
      );
      let cInDegrees = this.radiansToDegrees(cInRadians);

      // get Degree for A
      let aInRadians = Math.acos(
        (Math.pow(b, 2) + Math.pow(c, 2) - Math.pow(a, 2)) / (2 * b * c)
      );

      let aInDegrees = this.radiansToDegrees(aInRadians);

      // get Degree for A
      let bInRadians = Math.acos(
        (Math.pow(c, 2) + Math.pow(a, 2) - Math.pow(b, 2)) / (2 * c * a)
      );

      let bInDegrees = this.radiansToDegrees(bInRadians);

      // the the angles
      this.angleA = aInDegrees;
      this.angleB = bInDegrees;
      this.angleC = cInDegrees;

      console.log(this.angleC.toFixed(0) == 90);

      // set the triangle angle type
      if (
        this.angleA.toFixed(0) < 90 &&
        this.angleB.toFixed(0) < 90 &&
        this.angleC.toFixed(0) < 90
      ) {
        this.angleType = "Acute";
      } else if (
        this.angleA.toFixed(0) > 90 ||
        this.angleB.toFixed(0) > 90 ||
        this.angleC.toFixed(0) > 90
      ) {
        this.angleType = "Obtuse";
      } else if (
        this.angleA.toFixed(0) == 90 ||
        this.angleB.toFixed(0) == 90 ||
        this.angleC.toFixed(0) == 90
      ) {
        this.angleType = "Right";
      }
    },
    radiansToDegrees(radians) {
      let pi = Math.PI;
      return radians * (180 / pi);
    },
    isIsoceles(a, b, c) {
      if (a == b && a != c) {
        return true;
      } else if (a == c && a != b) {
        return true;
      } else if (b == c && b != a) {
        return true;
      } else {
        return false;
      }
    },
    drawTriangle(a, b, c) {
      // just resets the key so that the component will redraw with the updated info
      this.triangleCanvasKey = a + b + c;
    },
  },
};
</script>
