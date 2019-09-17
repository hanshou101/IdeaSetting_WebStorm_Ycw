<!--制作注意：在模板文件里，用${DS}{DS}来表示单纯美元符号，用${DS}name来指代变量（注意一个词：迭代）-->
<!--TIP注释的使用：①-重要信息和描述 ②-适合给方法加注释 ③-不要过度使用，不要给HTML标签TIP加注释 ④-  -->

<template>
  <div class="common-main">

    <!--总表单-->
    <el-form :model="ruleForm" :rules="rules" ref="ruleFormRef" label-width="100px"
             class="search-container">

      <!--普通文本输入值-->
      <el-form-item :label="${DS}t('你的国际化语言地址')" prop="对应的表单校验值">
        <el-input v-model="ruleForm.的某个值" class="form-input" clearable></el-input>
      </el-form-item>

      <!--操作按钮组：搜索、新建、删除-->
      <div class="operation-container">
        <!--搜索-->
        <el-button icon="el-icon-search" @click="submitForm('ruleFormRef')">
          {{this.${DS}t('form.Search')}}
        </el-button>
        <!--新建-->
        <el-button type="primary" icon="el-icon-edit" @click="handleCreate">
          {{this.${DS}t('form.Create')}}
        </el-button>
        <!--删除-->
        <el-button type="danger" icon="el-icon-delete" @click="handleDelete">
          {{this.${DS}t('form.Delete')}}
        </el-button>
      </div>

    </el-form>

    <!--总表格-->
    <el-table
      ref="multipleTable"
      :data="listData"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
      v-loading="listLoading">

      <!--表格，复选框。影响到this.deleteItems的列表值。-->
      <el-table-column
        type="selection"
        width="55">
      </el-table-column>

      <!--普通字段-->
      <el-table-column
        :label="${DS}t('你的国际化语言地址')"
        prop="接口数据的对应字段名"
        min-width="100">
      </el-table-column>


      <!--彩色标签字段-->
      <el-table-column
        :label="${DS}t('你的国际化语言地址')"
        prop="接口数据的对应字段名_对应五大语言"
        width="300">
        <template slot-scope="scope">
          <MultiLangTag :data="scope.row.接口数据的对应字段名_对应五大语言"></MultiLangTag>
        </template>
      </el-table-column>


      <!--常用的Tag状态字段-->
      <el-table-column
        :label="${DS}t('table.Status')"
        prop="接口数据的对应字段名_状态Status">

        <template slot-scope="scope">
          <el-tag :type="scope.row.接口数据的对应字段名_状态Status| elTagFilter">
            {{scope.row.接口数据的对应字段名_状态Status | 某一个StatusFilter_常在common_mixin中出现}}
          </el-tag>
        </template>

      </el-table-column>

      <!--表格内某行，操作按钮组：编辑、启用、禁用-->
      <el-table-column
        :label="${DS}t('table.Operation')"
        width="200">

        <template slot-scope="scope">

          <!--编辑-->
          <el-button
            size="mini"
            type="primary"
            @click="handleEdit(scope.${DS}index, scope.row)" icon="el-icon-edit">
            {{${DS}t('table.Edit')}}
          </el-button>

          <!--启用-->
          <el-button
            size="mini"
            v-show="scope.row.status===0"
            @click="handleChangeStatus(scope.${DS}index, scope.row)" icon="el-icon-edit">
            {{${DS}t('table.Enable')}}
          </el-button>

          <!--禁用-->
          <el-button
            size="mini"
            type="danger"
            v-show="scope.row.status===1"
            @click="handleChangeStatus(scope.${DS}index, scope.row)" icon="el-icon-edit">
            {{${DS}t('table.Disable')}}
          </el-button>
          <!---->
        </template>

      </el-table-column>

    </el-table>

    <!--分页器。当选中页码更改时，会立即触发_getList刷新。-->
    <el-pagination
      class="pagination-container"
      background
      layout="total,prev, pager, next"
      :current-page.sync="listQuery.current"
      :page-size="listQuery.size"
      :total="listQuery.total"
      @current-change="handlePageChange">
    </el-pagination>

    <!--Dialog总控制器。当触发【新建】、【更改】操作时，会触发父组件的_getList刷新。-->
    <!--TODO 解开这里，即可召唤神龙实现你的愿望。-->
    <!--<Your_Dialog :ref="'dialogRef'"-->
    <!--v-if="dialogControlVisible"-->
    <!--@refreshList="_getList"-->
    <!--@closeDialogEvent="closeDialogEvent">-->
    <!--</Your_Dialog>-->

    <!--冒险的中场休息-->
  </div>
</template>

<script>
  import commonMixin from '@/components/mixin/common-mixin'       // 绝对路径。很稳定。
  import { mapGetters, } from 'vuex'                                 // 状态机模块。

  // API，建议采用相对路径。
  // import {paramConfigApi} from "@/api/paramConfigApi"

  // Dialog，建议采用相对路径。
  // import Your_Dialog from './component/你的Dialog'

  import MultiLangTag from '@/components/MultiLang/MultiLangTag'        // 自定义组件：五大语种语言，彩色标签控件。

  export default {
    name: '',
    components: {
      MultiLangTag,
      // Your_Dialog       // 你的Dialog，需要配置import导入。
    },
    filters: {},
    mixins: [ commonMixin, ],
    props: {},
    data() {
      return {
        rules: {
          // 可通过Live Template，生成rules。
        },
        ruleForm: {
          // 你的列表查询提交数据
        },
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
      // TIP 列表接口
      listCallback() {
        // return 列表查询接口.list(this.ruleForm,this.listQuery.current, this.listQuery.size)
      },
      // TIP 更改Status接口
      changeStatusCallback(id, status) {
        // return 更新单个状态接口.changeStatus(id,status)
      },
      // TIP 删除接口
      deleteCallback(ids) {
        // return 删除多个接口.delete(ids)
      },
      // TIP 方法已过时：已更改mixin的抽取方法。
      // handleCreate() {
      //  // this.dialogControlVisible = true   //开始渲染Dialog总控制器
      //  // this.${DS}nextTick(() => {  this.${DS}refs.dialogRef.showDialog(1)  })    //对话框-新建内容，showDialog只带1个参数数字【1】。
      // },
      // TIP 方法已过时：已更改mixin的抽取方法。
      // handleEdit(index, row) {
      //  // this.dialogControlVisible = true   //开始渲染Dialog总控制器
      //  // this.${DS}nextTick(() => {  this.${DS}refs.dialogRef.showDialog(2, Object.assign({}, row))  })   //对话框-编辑内容。将当前行row的内容，带去对话框。
      // }
    },
  }
</script>

<style scoped>

</style>
