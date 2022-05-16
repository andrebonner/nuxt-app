<template>
  <span class="menu">
    <nuxt-link to="/polls">Polls</nuxt-link>
  </span>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "IndexPage",
});
</script>
<style lang="scss">
.menu {
  background-color: transparent;
  color: $colorPrimary;
  padding: 1rem;
  border: 1px solid $colorPrimaryDark;
  border-radius: 4px;
  .menu a {
    margin: 0;
    padding: 0;
    width: 500px;
  }
  &:hover {
    cursor: pointer;
    background-color: $colorPrimary;
    color: #ddd;
  }
}
</style>
