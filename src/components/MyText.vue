<template>
  <div @click="gotClicked">{{text}}</div>
</template>
<script>
export default {
  text: "A tips",
  props: {
    text: String
  },
  methods: {
    gotClicked: function() {
      this.$emit("was-clicked", this.text + " - " + new Date());
    }
  }
};
</script>
