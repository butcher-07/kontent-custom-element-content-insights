<template>
  <v-flex>
    <strong>{{ score.notes }}</strong>
  </v-flex>
</template>
<script>
import readability from "readability-meter";

export default {
  props: ["value"],
  data() {
    return {
      score: {}
    };
  },
  watch: {
    value() {
      if (this.value) this.score = this.value;
    }
  },
  created() {
    this.$eventBus.$on("newContentReady", this.analyzeContent);
  },
  methods: {
    analyzeContent(content) {
      var startTime = new Date();
      this.score = this.analyzeReadibility(content);
      
      var endTime = new Date();

            console.log("Readability: " + ((endTime - startTime) / 1000) + " seconds");
      this.$emit("save", this.score);
    },
    analyzeReadibility(text) {
      return readability.ease(text);
    }
  }
};
</script>
<style scoped>
div {
  margin-left: 15px;
}
em {
  margin-left: 10px;
}
</style>
