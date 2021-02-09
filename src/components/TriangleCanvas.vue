<template>
  <v-stage ref="stage" :config="stageSize">
    <v-layer>
      <v-shape
        v-if="show"
        :config="{
          sceneFunc: sceneFunc,
          fill: '#00D2FF',
          stroke: 'black',
          strokeWidth: 2,
        }"
      />
    </v-layer>
  </v-stage>
</template>

<script>
const width = 500;
const height = 500;

export default {
  name: "TriangleCanvas",
  props: {
    sideA: Number,
    sideB: Number,
    sideC: Number,
    angleA: Number,
    angleB: Number,
    angleC: Number,
    show: Boolean,
  },
  data() {
    return {
      stageSize: {
        width: width,
        height: height,
        lenA: 0,
        lenB: 0,
        lenC: 0,
      },
    };
  },
  mounted() {
    // if (this.sideA > this.width || this.sideB > this.width || this.sideC > this.width) {
    //   this.scaleSides()
    // } else if (this.sideA < 100 || this.sideB < 100 || this.sideC < 100) {
    //   this.sideA = this.sideA * 10;
    //   this.sideB = this.sideB * 10;
    //   this.sideC = this.sideC * 10;
    // }
    this.scaleSides();
  },
  methods: {
    sceneFunc(context, shape) {
      context.beginPath();
      context.moveTo(0, 0);
      context.lineTo(this.lenA + 0, 0);
      context.lineTo(
        this.lenA +
          this.lenB * Math.cos(this.degreesToRadians(180 - this.angleC)),
        this.lenB * Math.sin(this.degreesToRadians(180 - this.angleC))
      );
      context.closePath();

      context.fillStrokeShape(shape);
    },
    degreesToRadians(degrees) {
      let pi = Math.PI;
      return degrees * (pi / 180);
    },
    scaleSides() {
      let sideList = [
        parseInt(this.sideA),
        parseInt(this.sideB),
        parseInt(this.sideC),
      ];
      sideList.sort();
      let longestSide = sideList[sideList.length - 1];
      console.log(sideList);

      let scaleFactor = longestSide / width;

      this.lenA = parseInt(this.sideA) / scaleFactor;
      this.lenB = parseInt(this.sideB) / scaleFactor;
      this.lenC = parseInt(this.sideC) / scaleFactor;
    },
  },
};
</script>
