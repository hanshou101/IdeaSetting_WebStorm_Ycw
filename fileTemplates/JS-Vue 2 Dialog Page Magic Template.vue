<!--制作注意：在模板文件里，用${DS}{DS}来表示单纯美元符号，用${DS}name来指代变量（注意一个词：迭代）-->
<!--TIP注释的使用：①-重要信息和描述 ②-适合给方法加注释 ③-不要过度使用，不要给HTML标签TIP加注释 ④-  -->

<template>

  <!--Controller内部的Dialog对话框。当触发@close事件时，this.dialogVisible变为false。而且经由Controller，触发父组件的closeDialogEvent事件-->
  <el-dialog
    :close-on-click-modal="false"
    :title="dialogTitle"
    :visible.sync="dialogVisible"
    @close="closeDialog"
    width="60%">

    <!--总表单-->
    <el-form :model="ruleForm" :rules="rules" ref="ruleFormRef" label-width="180px"
             class="dialog-container">

      <!--普通文本输入值-->
      <el-form-item :label="${DS}t('你的国际化语言地址')" prop="对应的表单校验值">
        <el-input v-model="ruleForm.的某个值" class="form-input" clearable></el-input>
      </el-form-item>

      <el-form-item :label="${DS}t('你的国际化语言地址')" prop="对应的表单校验值">
        <el-select v-model="绑定某个Form.的某个值"
                   class="form-input" clearable>
          <el-option v-for="(value_obj,key_index) in 可选择列表_必须是列表_item元素_index索引  "
                     :key="key_index"
                     :label="value_obj.标签名"
                     :value="parseInt( value_obj.选项真值 | 此处，切记注意，太长的数字ID，比如币种ID，会丢失精度，所以太长的话就禁止使用parseInt )"></el-option>
        </el-select>
      </el-form-item>

      <MultiLangPlus :ruleForm="ruleForm" :rules="rules" :ruleFormRef="ruleFormRef"
                 :labelName="labelName"></MultiLangPlus>
      <!--。-->
    </el-form>

    <!--对话框按钮组：确定、取消-->
    <span slot="footer" class="dialog-footer">
        <!--取消按钮。将会把this.dialogVisible值变为false-->
        <el-button @click="dialogVisible = false">
          {{${DS}t('dialog.Cancel')}}
        </el-button>

      <!--确定按钮。submitForm方法提交ruleFormRef表单。-->
        <el-button @click="submitForm('ruleFormRef')"
                   type="primary">
          {{${DS}t('dialog.Confirm')}}
        </el-button>
    </span>

    <!--冒险的中场休息-->
  </el-dialog>
</template>

<script>
  import dialogMixin from '@/components/mixin/dialog-mixin'       // 绝对路径。很稳定。
  import {mapGetters} from 'vuex'                                 // 状态机模块。
  import MultiLangPlus from '@/components/MultiLang/MultiLangPlus'                  // 自定义组件：万能翻译机。


  // API，建议采用相对路径。
  // import {paramConfigApi} from '@/api/paramConfigApi'


  export default {
    name:'',
    components: {
      MultiLangPlus,
    },
    filters: {},
    mixins: [dialogMixin],
    props: {},
    data() {
      return {
        labelName: '这里是对于_MultiLang_组件的标签描述_作为prop传入',
        ruleFormRef: null,            // 将会传入【MultiLang】组件，拼接成为一整个【Form】整体。
        dialogTitle: '这里是_Dialog对话框的标题',
        rules: {
          // 可通过Live Template，生成rules。
        },
        ruleForm: {
          // 你的列表查询提交数据。（也将会和【MultiLang】组件内部，进行并组）
        }
      }
    },
    computed: {
      ...mapGetters([]),
    },
    watch: {
      // TIP 切记，watch里面的最外层声明，不能使用箭头函数。（使用了会报错；反而，不使用不会影响this的使用。）
      // TIP deep属性为true，将会同时监听对象内部值的变化。
      // TIP immediate属性，将会立即以表达式的当前值，触发一次回调。（一般用于，监听props在初始化时传入的数据。）                参考资料：https://cn.vuejs.org/v2/api/#vm-watch
      // TIP 而对于那些由网络请求拿到的异步传入的props，普通的watch，就可以充分地监听到数据。（反而是一开始初始化时就完善的props，需要用immediate属性来监听。）

      dialogType: {
        immediate: true,
        handler: function(val) {
          if (val === 1) {
            this.dialogTitle = this.${DS}t('dialog.Create')       // 可以说明，新建的具体内容
          } else {
            this.dialogTitle = this.${DS}t('table.Edit')       // 可以说明，编辑的具体内容
          }
        },
      },

    },
    created() {
    },
    mounted() {
    },
    updated() {
    },
    activated() {
    },
    destroyed() {
    },
    methods: {
      // TIP 对话框：新建模式，刚进入时的回调。
      addCallback() {
        this.${DS}nextTick(() => {
          this.ruleFormRef = this.${DS}refs.ruleFormRef         // 将对话框的Form引用，传入【MultiLang】
        })
      },
      // TIP 对话框：编辑模式，刚进入时的回调。
      editCallback(row) {
        this.${DS}nextTick(() => {
          this.ruleFormRef = this.${DS}refs.ruleFormRef         // 将对话框的Form引用，传入【MultiLang】
        })
      },
      // TIP 新增接口
      createCallback() {
        // return 新增单个接口.create(this.ruleForm)
      },
      // TIP 编辑（更新）接口
      updateCallback() {
        // return 更新单个接口.update(this.ruleForm)
      },
      // TIP 对话框关闭回调
      closeCallback() {
        // 此回调，可能已Deprecated。
      },
    }
  }
</script>

<style scoped>

</style>
