<template>
  <div>
    <p id="result-view">{{ result }}</p>
  </div>
</template>

<script>
export default {
  props: ["result"],
  mounted() {
    this.$emit('mountedResultView');
  }
}
</script>

<style>
#result-view {
  width: 90%;
  min-height: 80px;
  font-size: 16px;
  background: #fff;
  margin-bottom: 20px;
  color: #333;
  border: 1px solid #ddd;
  border-radius: 3px;
}
</style>
