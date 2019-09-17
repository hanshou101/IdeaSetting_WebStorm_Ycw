<!--制作注意：在模板文件里，用${DS}{DS}来表示单纯美元符号，用${DS}name来指代变量（注意一个词：迭代）-->
<!--TIP注释的使用：①-重要信息和描述 ②-适合给方法加注释 ③-不要过度使用，不要给HTML标签TIP加注释 ④-  -->

<template>
  <div class="block__element--modifier common-main">

    <!--总表单-->
    <el-form :model="ruleForm" :rules="rules" ref="ruleFormRef" label-width="100px"
             class="search-container">

      <MyFormEasy :form-items="formItems" :ruleForm="ruleForm"></MyFormEasy>

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
        style="width: 100%;"
        @selection-change="handleSelectionChange"
        v-loading="listLoading">

      <MyTableEasy :table-cols="tableCols"></MyTableEasy>

      <!--表格内某行，操作按钮组：编辑、启用、禁用-->
      <el-table-column
          :label="${DS}t('table.Operation')"
          fixed="right"
          align="center"
          width="80">

        <template slot-scope="scope">

          <Dropdown>

          <!--编辑-->
          <el-button
              slot="dropdown-item"
              size="mini"
              type="primary"
              @click="handleEdit(scope.${DS}index, scope.row)" icon="el-icon-edit">
            {{${DS}t('table.Edit')}}
          </el-button>

          <!--启用-->
          <el-button
              slot="dropdown-item"
              size="mini"
              v-if="scope.row.status===0"
              @click="handleChangeStatus(scope.${DS}index, scope.row)" icon="el-icon-edit">
            {{${DS}t('table.Enable')}}
          </el-button>

          <!--禁用-->
          <el-button
              slot="dropdown-item"
              size="mini"
              type="danger"
              v-if="scope.row.status===1"
              @click="handleChangeStatus(scope.${DS}index, scope.row)" icon="el-icon-edit">
            {{${DS}t('table.Disable')}}
          </el-button>
          <!---->

          </Dropdown>

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

<script lang="ts">

  @MyComponent({
    name: "${NAME}",
    components: {
      /*组件*/
      MultiLangTag,
      MyFormEasy,
      MyTableEasy,
    },
    filters: {},
  })
  export default class ${NAME}
    extends AbstractBaseVueClass_NotDirectExtendedByMixins.Mixins(CommonMixin) {    // 混入在此处，进行添加。

    // TIP————————————————————————————————————————初始化Mixin的上下级关系————————————————————————————
    initFunc() {
      const self = this;

      this.MixinsData_1 = {...this.MixinsData_1};       // TODO 顶级Mixins的数据初始化
      this.MixinsData_2 = {                             // TODO 二级Mixins的数据初始化
        ...this.MixinsData_2,
        lang: "",                       // 后台管理系统，当前界面语种
        listCallback() {                                            // 菜单第一时间，自动拉取的列表接口
          // return ArticleApi.list(self.ruleForm, self.listQuery.current, self.listQuery.size);
        },
        needListProcess: (res: any) => {                            // 如果，在列表接口之后，需要对返回的数据进行【预处理】
          console.log("接口列表预处理");
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

    // TIP————————————————————————————————————Data，在类中的实现（@Model相关的除外）——————————————————————
    ruleForm = {};
    get rules(): MyEl_FormItem_Rule_Config {
      return {
        // 类型
        // category: [new MyEl_RuleItem(this.${DS}t("websiteOperation.articleManager.Select_Category").toString(),)],
      }
    }//
    get formItems(): MyEl_FormItem[] {
      return [
        new MyEl_FormItem('context', 'Context'),
        // new MyEl_FormItem('coinId', '币种ID', 'options', '', new MyFormItem_SelectOptionConf({1: 'BTC'})),
        // new MyEl_FormItem('dateRange', '时间范围', 'date_time'),
      ];
    }//
    get tableCols(): MyElement_TableCol[] {
      return [                                                    // TIP 现在已经修复了这一BUG  // TODO 因为ElementUI的Bug，需要将最后一个条目，移到数组的第一个。
        new MyElement_TableCol('lastUpdateTime', '更新时间', '160'),      // TIP 注意，如果未设置:key="动态生成唯一key"，这其实是最后一个Item；如果设置了  :key="动态生成唯一key"，则这是第一个Item。
        //
        new MyElement_TableCol('id', 'ID', '100'),
      ];
    }//

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

  }

</script>

<style rel="stylesheet/scss" lang="scss" scoped>

  // 尝试一些特别的命名法。比如【BEM】
  .block__element--modifier{

  }

</style>
