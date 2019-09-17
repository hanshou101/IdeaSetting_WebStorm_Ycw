<!--制作注意：在模板文件里，用${DS}{DS}来表示单纯美元符号，用${DS}name来指代变量（注意一个词：迭代）-->
<!--TIP注释的使用：①-重要信息和描述 ②-适合给方法加注释 ③-不要过度使用，不要给HTML标签TIP加注释 ④-  -->
<template>
  <div class="block__element--modifier common-main">
    <!--总表单-->
    <el-form :model="ruleForm" :rules="rules" ref="ruleFormRef"
             label-width="100px" class="search-container">
      <MyFormEasy :form-items="formItems" :ruleForm="ruleForm"></MyFormEasy>
      <div class="operation-container"><!--操作按钮组：搜索、新建、删除-->
        <el-button icon="el-icon-search" @click="submitForm('ruleFormRef')">{{this.${DS}t('form.Search')}}</el-button><!--搜索-->
        <el-button type="primary" icon="el-icon-edit" @click="handleCreate">{{this.${DS}t('form.Create')}}</el-button><!--新建-->
        <el-button type="danger" icon="el-icon-delete" @click="handleDelete">{{this.${DS}t('form.Delete')}}</el-button><!--删除-->
      </div>
    </el-form>

    <!--总表格-->
    <el-table :data="listData" @selection-change="handleSelectionChange" v-loading="listLoading"
        tooltip-effect="dark"
        ref="multipleTable"
        class="element-table-list" style="width: 100%;">
      <MyTableEasy :table-cols="tableCols">
        <template #operate="scope">
          <!--<Dropdown>
            &lt;!&ndash;编辑&ndash;&gt;
            <el-button @click="handleEdit(scope.${DS}index, scope.row)"  slot="dropdown-item" size="mini" type="primary"icon="el-icon-edit">{{${DS}t('table.Edit')}}</el-button>
            &lt;!&ndash;启用&ndash;&gt;
            <el-button v-if="scope.row.status===0" @click="handleChangeStatus(scope.${DS}index, scope.row)" slot="dropdown-item" size="mini" icon="el-icon-edit">{{${DS}t('table.Enable')}}</el-button>
            &lt;!&ndash;禁用&ndash;&gt;
            <el-button v-if="scope.row.status===1" @click="handleChangeStatus(scope.${DS}index, scope.row)" slot="dropdown-item" size="mini" type="danger" icon="el-icon-edit">{{${DS}t('table.Disable')}}</el-button>
          </Dropdown>-->

          <!--编辑-->
          <el-button @click="handleEdit(scope.${DS}index, scope.row)"  slot="dropdown-item" size="mini" type="primary"icon="el-icon-edit">{{${DS}t('table.Edit')}}</el-button>
          <!--启用-->
          <el-button v-if="scope.row.status===0" @click="handleChangeStatus(scope.${DS}index, scope.row)" slot="dropdown-item" size="mini" icon="el-icon-edit">{{${DS}t('table.Enable')}}</el-button>
          <!--禁用-->
          <el-button v-if="scope.row.status===1" @click="handleChangeStatus(scope.${DS}index, scope.row)" slot="dropdown-item" size="mini" type="danger" icon="el-icon-edit">{{${DS}t('table.Disable')}}</el-button>
        </template>
      </MyTableEasy>
    </el-table>

    <!--分页器。当选中页码更改时，会立即触发_getList刷新。-->
    <el-pagination :current-page.sync="listQuery.current" :page-size="listQuery.size" :total="listQuery.total" @current-change="handlePageChange"
        layout="total,prev, pager, next"
        background
        class="pagination-container"/>

    <!--Dialog总控制器。当触发【新建】、【更改】操作时，会触发父组件的_getList刷新。-->

    <!--TODO 解开这里，即可召唤神龙实现你的愿望。-->
    <!--<YourDialog
        ref="dialogRef"
        v-if="dialogVisible"
        :show.sync="dialogVisible"
        :init-data="initData"
        @refreshList="_getList">
    </YourDialog>-->
  </div>
</template>

<script lang="ts">
  import {MyEl_FormItem, MyEl_FormItem_Rule_Config} from "@/_framework/sdk/elment-ui/MyElementUtils";
  import BaseVue, {MyComponent} from "@/_framework/BaseVue";
  import CommonMixin from "@/_mixin/common-mixin";
  @MyComponent({
    name: "${NAME}",
    components: { /*组件*/

    },
    filters: {},
  })
  export default class ${NAME}
    extends BaseVue.Mixins(CommonMixin) {    // 混入在此处，进行添加。
    // TIP————————————————————————————————————Prop，从外界传入的只读属性—————————————————————————————————

    // TIP————————————————————————————————————Data，在类中的实现（@Model相关的除外）——————————————————————
    ruleForm = {};

    // TIP————————————————————————————————————computed，在类中的实现（@Model相关的除外）——————————————————————
    get rules(): MyEl_FormItem_Rule_Config {
      return { // 示范
        // demo: [new MyEl_RuleItem(this.${DS}t("websiteOperation.articleManager.Select_Category").toString(),)],
      }
    }//
    get formItems(): MyEl_FormItem[] {
      return [
        new MyEl_FormItem('context', 'Context'),
        // new MyEl_FormItem('coinId', '币种ID', 'options', '', new MyFormItem_SelectOptionConf({1: 'BTC'})),
        // new MyEl_FormItem('dateRange', '时间范围', 'date_time'),
      ];
    }//
    get tableCols(): any[] {
      return [
        {name: 'id', prop: 'id', type: 'text', label: 'ID'},
        {name: 'lastUpdateTime', prop: 'lastUpdateTime', type: 'text', label: '更新时间'},
        {name: 'status', prop: 'status',  label: '状态', type: "enumTag", selectOption: this.selectOption.status},
        {
          name: 'operate', /*label: '操作', */ i18nKey: 'table.Operation', minWidth: 120,
          elementTableColumnAttrs: {
            fixed: "right",
            align: "center"
          }
        },
      ];
    }

    // TIP——————————————————————————————————————Method，在类中的实例——————————————————————————————————————

    // TIP——————————————————————————————————————Vue生命周期，在类中的实现——————————————————————————————————
    created(): void {
      this.initFunc();
    };

    mounted(): void {
    };

    activated(): void {
    };

    updated(): void {
    };

    destroyed(): void {
    };

    // TIP————————————————————————————————————————初始化Mixin的上下级关系————————————————————————————
    initFunc() {
      this.MixinsData_1 = {...this.MixinsData_1};       // TODO 顶级Mixins的数据初始化
      this.MixinsData_2 = {                             // TODO 二级Mixins的数据初始化
        ...this.MixinsData_2,
        lang: "",                       // 后台管理系统，当前界面语种
        listCallback: () => {                                            // 菜单第一时间，自动拉取的列表接口
          // return ArticleApi.list(this.ruleForm, this.listQuery.current, this.listQuery.size);
        },
        needListProcess: (res: any) => {                            // 如果，在列表接口之后，需要对返回的数据进行【预处理】
          return res;                                                       // 注意，若没有预处理过程，则返回原始数据即可。
        },
        changeStatusCallback(id: string, status: string) {          // 更新单个菜单条目的状态（如禁用、启用）的接口
          // return ArticleApi.changeStatus({id, status,});
        },
        deleteCallback(ids: any) {                                  // 删除菜单条目的接口
          // return ArticleApi.delete(ids);
        },
      };
    }

  }

</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  // 尝试规范的进行  CSS开发。
</style>
