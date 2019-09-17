<template>
    <section class="${dash_name}">
    </section>
</template>

<script lang="ts">
  import Vue from 'vue';
  
  export default Vue.extend({
      name: "${NAME}",
      components: {},
      filters: {},
      mixins: [],
      props: {},
      data () {
        return {};
      },
      computed: {
      },
      watch: {},
      mounted () {},
      methods: {},
  })   
</script>

<style lang="less" scoped>
  @import "./${dash_name}.scoped.less";
</style>