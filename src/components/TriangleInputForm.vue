<template>
  <v-container>
    <v-row>
      <div>
        <v-text-field
          type="number"
          v-model="sideA"
          label="Side A"
          placeholder="10"
        ></v-text-field>
      </div>
    </v-row>
    <v-row>
      <div>
        <v-text-field
          type="number"
          v-model="sideB"
          label="Side B"
          placeholder="10"
        ></v-text-field>
      </div>
    </v-row>
    <v-row>
      <div>
        <v-text-field
          type="number"
          v-model="sideC"
          label="Side C"
          placeholder="21"
        ></v-text-field>
      </div>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "TriangleInputForm",
  data() {
    return {
      sideA: null,
      sideB: null,
      sideC: null,
    };
  },
  watch: {
    sideA(newA) {
      this.$emit("change", { a: newA, b: this.sideB, c: this.sideC });
    },
    sideB(newB) {
      this.$emit("change", { a: this.sideA, b: newB, c: this.sideC });
    },
    sideC(newC) {
      this.$emit("change", { a: this.sideA, b: this.sideB, c: newC });
    },
  },
};
</script>
