<template>
  <h1>Result of your Test</h1>
  <p>Total Tab count{{ code.TabSwitchCount }}</p>
  <p>Your code is {{ code.codewrite }}</p>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      code: {
        codewrite: "",
        TabSwitchCount: "",
      },
    };
  },
  methods: {
    async displyResult() {
      const codedisply = await axios.get("http://localhost:3000/code");
      this.code = codedisply.data;
      console.log("result", this.code);
      this.code.codewrite = codedisply.data[0].codewrite;
      this.code.TabSwitchCount = codedisply.data[0].TabSwitchCount;
    },
  },
  mounted() {
    this.displyResult();
  },
};
</script>

<style></style>
