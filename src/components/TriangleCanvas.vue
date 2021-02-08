<template>
  <v-stage ref="stage" :config="stageSize">
    <v-layer>
      <v-shape
        :config="{
          sceneFunc: function(context, shape) {
            context.beginPath();
            context.moveTo(0, 0);
            context.lineTo(sideA, 0);
            context.lineTo(sideA, sideB);
            context.closePath();

            // special Konva.js method
            context.fillStrokeShape(shape);
          },
          fill: '#00D2FF',
          stroke: 'black',
          strokeWidth: 4,
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
  },
  data() {
    return {
      stageSize: {
        width: width,
        height: height,
      },
    };
  },
  mounted() {
    console.log("reload");
  },
};
</script>
