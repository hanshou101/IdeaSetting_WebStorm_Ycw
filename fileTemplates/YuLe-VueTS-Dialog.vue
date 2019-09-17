<!--制作注意：在模板文件里，用${DS}来表示单纯美元符号，用${DS}name来指代变量（注意一个词：迭代）-->
<!--TIP注释的使用：①-重要信息和描述 ②-适合给方法加注释 ③-不要过度使用，不要给HTML标签TIP加注释 ④-  -->
<template>
  <!--Controller内部的Dialog对话框。当触发@close事件时，this.dialogVisible变为false。而且经由Controller，触发父组件的closeDialogEvent事件-->
  <el-dialog  :title="dialogTitle" :visible.sync="dialogVisible" :close-on-click-modal="false" @close="closeDialog"
              class="common-dialog" width="60%">

    <!--总表单-->
    <el-form :model="MixinsData_2.ruleForm" :rules="rules" ref="ruleFormRef" label-width="120px" class="dialog-container">
      <MyFormEasy :form-items="formItems" :ruleForm="MixinsData_2.ruleForm"></MyFormEasy>
    </el-form>

    <!--对话框按钮组：确定、取消-->
    <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">{{${DS}t('dialog.Cancel')}}</el-button><!--取消按钮。将会把this.dialogVisible值变为false-->
        <el-button @click="submitForm('ruleFormRef')" type="primary">{{${DS}t('dialog.Confirm')}}</el-button><!--确定按钮。submitForm方法提交ruleFormRef表单。-->
    </span>

  </el-dialog>
</template><!--冒险的中场休息-->

<script lang="ts">

    @MyComponent({
        name: "${NAME}",
        components: { /*组件*/
            MultiLangPlus, MyFormEasy,
        },
        filters: {},
    })
    export default class ${NAME}
        extends BaseVue.Mixins(DialogMixin) {
        // TIP————————————————————————————————————Prop，从外界传入的只读属性—————————————————————————————————

        // TIP————————————————————————————————————Data，在类中的实现（@Model相关的除外）——————————————————————

        ruleFormRef: any = null;          // ElForm表单的${DS}ref引用
        dialogTitle: string = "";         // 稍候，在initFunc里面，进行初始化。（原因：用get方法的话，会变成只读；而稍候，存在需要修改的情况。）

        get rules(): MyEl_FormItem_Rule_Config {                      // 表单检验规则配置
          return { // 示范
            // demo: [new MyEl_RuleItem(this.${DS}t("websiteOperation.articleManager.Select_Category").toString(),)],
          }
        };

        get formItems(): MyEl_FormItem__inDialog[] {                  // 动态生成FormItem表单项
          return [
            new MyEl_FormItem__inDialog('context', 'Context'),
            // new MyEl_FormItem__inDialog('coinId', '币种ID', 'options', '', new MyFormItem_SelectOptionConf({1: 'BTC'})),
            // new MyEl_FormItem__inDialog('dateRange', '时间范围', 'date_time'),
          ];
        };

        // TIP————————————————————————————————————Computed，在类中的实现——————————————————————————————————————

        // TIP————————————————————————————————————Watch，在类中的实现—————————————————————————————————————————
        @MyWatch("dialogType", {immediate:true})
        public watch_dialogType(newVal: any, oldVal: any) {               // 新建/编辑 对话框的标题
          this.dialogTitle = newVal === 1 ? this.${DS}t('dialog.Create').toString() : this.${DS}t('table.Edit').toString()
        }

        // TIP————————————————————————————————————Method，在类中的实现————————————————————————————————————————

        // TIP————————————————————————————————————Vue生命周期，在类中的实现——————————————————————————————————
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


        // TIP————————————————————————————————————该方法，会在整个外部控制组件初始化，即放置在Html时，就执行；而showDialog方法，会在之后用户手动触发（伴随ElDialog的显示）。
        initFunc() {
          const self = this;
          this.dialogTitle = this.${DS}t('dialog.Create').toString();     // 放这儿初始化标题值。好像没有更加合适一点的地方了。
          this.MixinsData_1 = {...this.MixinsData_1};       // TODO 顶级Mixins的数据初始化
          this.MixinsData_2 = {                             // TODO 二级Mixins的数据初始化
            ...this.MixinsData_2,
            ruleForm: {
              // ... ...  子组件和它的Mixin，共用的表单属性
            },
            addCallback() {                                       // TIP 新建模式对话框的开启回调：将在showDialog的最后执行，作为附加额外操作。
              self.${DS}nextTick(() => {
                self.ruleFormRef = self.${DS}refs.ruleFormRef;      // 将对话框的Form引用，传入【MultiLang】
              });
            },
            editCallback(row: any) {                              // TIP 编辑模式对话框的开启回调：将在showDialog的最后执行，作为附加额外操作。
              self.${DS}nextTick(() => {
                self.ruleFormRef = self.${DS}refs.ruleFormRef;      // 将对话框的Form引用，传入【MultiLang】
              });
            },
            closeCallback() {                                     // TIP 对话框的关闭回调。（此回调，可能已Deprecated）
              console.log("关闭对话框");
            },
            createCallback() {                                    // 点击确定，调用新增接口。（新建模式下）
              // const form =filterTargetFields(self.MixinsData_2.ruleForm,CreateBean)
              // return ArticleApi.create(form);
            },
            updateCallback() {                                    // 点击确定，调用编辑接口。（编辑模式下）
              // const form =filterTargetFields(self.MixinsData_2.ruleForm,UpdateBean)
              // return ArticleApi.update(form);
            },
            processCreatedCallback(res: any) {                            // （较少用）如果你要在对话框关闭之前，对【创建接口】返回的数据，进行额外处理或与额外操作。
              return res;                                                 // 注意，若没有额外处理过程，则返回原始数据即可。
            },
          };
        }

    }

</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  // 尝试规范的进行  CSS开发。

</style>
