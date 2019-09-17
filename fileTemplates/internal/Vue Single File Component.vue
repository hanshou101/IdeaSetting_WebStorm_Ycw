<template>
#[[$END$]]#
</template>

<script>
export default {
  name: "${COMPONENT_NAME}",

  components: {
    /* 组件 */
  },
  filters: {},
  mixins: [
    /* 混入 */
  ],
  props: {},
  data () {
    return {
      // rules: {
      // 可通过Live Template，生成rules。
      // },
      // ruleForm: {
      // 你的列表查询提交数据。（也将会和【MultiLang】组件内部，进行并组）
      // }
    };
  },
  computed: {
    ...mapGetters([ /* 你的Vuex-getter们 */]),
  },
  watch: {
    // TIP 切记，watch里面的最外层声明，不能使用箭头函数。（使用了会报错；反而，不使用不会影响this的使用。）
    // TIP deep属性为true，将会同时监听对象内部值的变化。
    // TIP immediate属性，将会立即以表达式的当前值，触发一次回调。（一般用于，监听props在初始化时传入的数据。）                参考资料：https://cn.vuejs.org/v2/api/#vm-watch
    // TIP 而对于那些由网络请求拿到的异步传入的props，普通的watch，就可以充分地监听到数据。（反而是一开始初始化时就完善的props，需要用immediate属性来监听。）
    // TIP 如果不低于两个属性，则原本的方法，用handler属性，存放。

  },
  created () {
  },
  mounted () {
  },
  updated () {
  },
  activated () {
  },
  destroyed () {
  },
  methods: {},
}
</script>

<style lang="stylus" type="text/stylus" scoped>

</style>