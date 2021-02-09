<template>
  <v-stage ref="stage" :config="stageSize">
    <v-layer>
      <v-shape
        v-if="show"
        :config="{
          sceneFunc: function(context, shape) {
            context.beginPath();
            context.moveTo(0, 0);
            context.lineTo(sideA + 0, 0);
            context.lineTo(
              sideA + sideB * Math.cos(degreesToRadians(180 - angleC)),
              sideB * Math.sin(degreesToRadians(180 - angleC))
            );
            context.closePath();

            context.fillStrokeShape(shape);
          },
          fill: '#00D2FF',
          stroke: 'black',
          strokeWidth: 2,
        }"
      />
    </v-layer>
  </v-stage>
</template>

<script>
const width = 1000;
const height = 1000;

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
      },
    };
  },
  methods: {
    degreesToRadians(degrees) {
      let pi = Math.PI;
      return degrees * (pi / 180);
    },
  },
};
</script>
