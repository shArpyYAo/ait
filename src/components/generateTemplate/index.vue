<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h3>Init normal Template</h3>
    <el-button type="text" @click="dialogShow('formDialogShow', '生成查询表单代码')">generateQueryForm</el-button>
    <el-button type="text" @click="dialogShow('btnRowDialogShow', '生成按钮行代码')">generateBtnRow</el-button>
    <el-button type="text" @click="dialogShow('tableDialogShow', '生成表格代码')">generateTable</el-button>
    <div style="display: flex;">
      <div><formTemplate ref="formTemplate"></formTemplate></div>
      <div><btn-row-template ref="btnRowTemplate"></btn-row-template></div>
      <div><table-template ref="tableTemplate"></table-template></div>
      <div><el-dialog-outer ref="elDialogOuter"></el-dialog-outer></div>
    </div>
    <el-dialog
      :title="dialogTitle"
      :visible.sync="dialogVisible"
      width="50%">
      <showTemplate :templateInShow="templateInShow"></showTemplate>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="generate">复 制</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import formTemplate from './normalTemplate/formTemplate'
import showTemplate from './normalTemplate/showTemplate'
import btnRowTemplate from './normalTemplate/btnRowTemplate'
import tableTemplate from './normalTemplate/tableTemplate'
import elDialogOuter from './normalTemplate/el-dialog'

export default {
    name: 'generateTemplate',
    components: {
      formTemplate,
      showTemplate,
      btnRowTemplate,
      tableTemplate,
      elDialogOuter
    },
    props: {
        msg: String,
    },
    data() {
        return {
            dialogVisible: false,
            dialogTitle: '',
            templateShow: false,
            templateInShow: '',
            chart: null,
            height: '650px'
        }
    },
    mounted() {},
    methods: {
        generate() {
          this.dialogVisible = false
        },
        dialogShow(type, title) {
          this.dialogVisible = true
          this.templateShow = false
          this.dialogTitle = title
          if (type === 'formDialogShow') {
            this.templateInShow = this.$refs.formTemplate.generateFormTemplate()
          }
          if (type === 'btnRowDialogShow') {
            this.templateInShow = this.$refs.btnRowTemplate.generateBtnRowTemplate()
          }
          if (type === 'tableDialogShow') {
            this.templateInShow = this.$refs.tableTemplate.generateTableTemplate()
          }
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
