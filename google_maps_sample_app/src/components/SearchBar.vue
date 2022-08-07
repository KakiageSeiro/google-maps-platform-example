<template>
  <div>
    <input 
      id="search-bar"
      class="controles"
      type="text"
      placeholder="キーワード検索"
      v-model="keyword"
      @change="placeAutocomplete"
    >
  </div>
</template>

<script>
export default {
  data() {
    return {
      keyword: ""
    };
  },
  mounted() {
    this.$emit('mountedSearchBar');
  },
  methods: {
    placeAutocomplete() {
      this.$emit("inputKeyword", this.keyword)
    }
  }
};
</script>

<style>
#search-bar { 
  width: 80%;
  height: 40px;
  font-size: 16px;
  margin-top: 20px;
  border: 1px solid #ddd;
  border-radius:3px;
}
</style>
