<template>
  <div class="common-main">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="search-container">

      <el-form-item label="基本字段" prop="基本字段">
        <el-input v-model="ruleForm.基本字段" class="form-input" clearable></el-input>
      </el-form-item>

      <div class="operation-container">
        <el-button icon="el-icon-search" @click="submitForm('ruleForm')">搜索</el-button>
        <el-button type="primary" icon="el-icon-edit" @click="handleCreate">新建</el-button>
        <el-button type="danger" icon="el-icon-delete" @click="handleDelete">删除</el-button>
      </div>
    </el-form>

    <el-table
      ref="multipleTable"
      :data="listData"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
      v-loading="listLoading"
    >
<!--      <el-table-column-->
<!--        type="selection"-->
<!--        width="55">-->
<!--      </el-table-column>-->

      <el-table-column
        prop="title"
        label="标题"
        min-width="100"
      >
      </el-table-column>

      <el-table-column
        prop="status"
        label="状态"
        min-width="100"
      >
        <template slot-scope="scope">
          <el-tag :type="scope.row.status| elTagFilter">
            {{scope.row.status|statusFilter}}
          </el-tag>

        </template>
      </el-table-column>

      <el-table-column
        label="操作"
        width="200"
      >
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="primary"
            @click="handleEdit(scope.${DS}index, scope.row)" icon="el-icon-edit">编辑
          </el-button>
          <el-button
            size="mini"
            v-show="scope.row.status===0"
            @click="handleChangeStatus(scope.${DS}index, scope.row)" icon="el-icon-edit">启用
          </el-button>
          <el-button
            size="mini"
            type="danger"
            v-show="scope.row.status===1"
            @click="handleChangeStatus(scope.${DS}index, scope.row)" icon="el-icon-edit">禁用
          </el-button>
        </template>
      </el-table-column>

    </el-table>

    <el-pagination
      class="pagination-container"
      background
      layout="total,prev, pager, next"
      :current-page.sync="listQuery.current"
      :page-size="listQuery.size"
      :total="listQuery.total"
      @current-change="handlePageChange"
    >
    </el-pagination>

    <article-dialog ref="dialogRef" @refreshList="_getList"></article-dialog>

  </div>
</template>

<script>
    import { noticeApi, } from '@/api/noticeApi'
    import commonMixin from '@/components/mixin/common-mixin'

    export default {
        name: "${NAME}",
        components: { /*articleDialog,*/ },
        mixins: [ commonMixin, ],
        data() {
            return {
                rules: {
                    // title: [
                        // {required: true, message: '请输入标题', trigger: 'blur'},
                    // ],
                },
                ruleForm: {
                    // title: '',
                },
            }
        },
        mounted() {
            // this.${DS}notify.warning('test');
        },
        methods: {
            listCallback() {
                // return noticeApi.getNoticeList(this.ruleForm, this.listQuery.current, this.listQuery.size)
            },
            deleteCallback(ids) {
                // return noticeApi.deleteNotice(ids)
            },
            handleCreate() {
                this.${DS}refs.dialogRef.showDialog(1, {})
            },
            handleEdit(index, row) {
                this.${DS}refs.dialogRef.showDialog(2, Object.assign({}, row))
            },
            async changeStatusCallback(id, status) {
                // return await noticeApi.updateStatus(id, status)
            },
        },
    }
</script>
