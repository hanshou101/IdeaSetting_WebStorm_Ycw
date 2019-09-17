<template>
  <el-dialog
    :title="dialogTitle"
    :visible.sync="dialogVisible"
    width="50%"
    @close="closeDialog"
  >
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px">

      <el-form-item label="基本字段" prop="基本字段">
        <el-input v-model="ruleForm.基本字段" class="form-input"></el-input>
      </el-form-item>

    </el-form>

    <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="submitForm('ruleForm')">确 定</el-button>
    </span>
  </el-dialog>
</template>

<script>
    import { noticeApi, } from '@/api/noticeApi'
    import dialogMixin from '@/components/mixin/dialog-mixin'

    export default {
        name: '${NAME}',
        components: { /*Tinymce,*/ },
        mixins: [ dialogMixin, ],
        data() {
            return {
                dialogTitle: '新建',
                rules: {
                    // title: [
                    //   { required: true, message: '请输入标题', trigger: 'blur', },
                    // ],
                },
                ruleForm: {},
            }
        },
        watch: {
            dialogType(val) {
                if (val === 1) {
                    this.dialogTitle = '新建'
                } else {
                    this.dialogTitle = '编辑'
                }
            },
        },
        mounted() {
            // this.${DS}notify.warning('test')
        },
        methods: {
            editCallback() {
                this.${DS}nextTick(() => {
                    // this.${DS}refs.tinymce.setContent(this.ruleForm.content)
                })
            },
            closeCallback() {
                // this.${DS}refs.tinymce.setContent('')
            },
            createCallback() {
                // return noticeApi.createNotice(this.ruleForm)
            },
            updateCallback() {
                // return noticeApi.updateNotice(this.ruleForm)
            },

        },
    }
</script>

<style scoped>

</style>


