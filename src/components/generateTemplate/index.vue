<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Init normal Template</h3>
    <el-button type="text" @click="dialogShow('formDialogShow', '生成查询表单代码')">generateQueryForm</el-button>
    <el-button type="text" @click="dialogShow('btnRowDialogShow', '生成按钮行代码')">generateBtnRow</el-button>
    <el-button type="text" @click="dialogShow('tableDialogShow', '生成表格代码')">generateTable</el-button>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
    <el-dialog
      :title="dialogTitle"
      :visible.sync="dialogVisible"
      width="50%">
      <formTemplate ref="formTemplate" v-if="dialogObj.formDialogShow"></formTemplate>
      <btn-row-template ref="btnRowTemplate" v-if="dialogObj.btnRowDialogShow"></btn-row-template>
      <table-template ref="tableTemplate" v-if="dialogObj.tableDialogShow"></table-template>
      <showTemplate :templateInShow="templateInShow" v-if="templateShow"></showTemplate>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = dialogObj.formDialogShow= dialogObj.btnRowDialogShow = false">取 消</el-button>
        <el-button type="primary" @click="generate">生 成</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import formTemplate from './normalTemplate/formTemplate'
import showTemplate from './normalTemplate/showTemplate'
import btnRowTemplate from './normalTemplate/btnRowTemplate'
import tableTemplate from './normalTemplate/tableTemplate'

export default {
    name: 'generateTemplate',
    components: {
      formTemplate,
      showTemplate,
      btnRowTemplate,
      tableTemplate
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
            dialogObj: {
                formDialogShow : false,
                btnRowDialogShow: false,
                tableDialogShow: false
            }
        }
    },
    methods: {
        generate() {
            const dialogArr = Object.keys(this.dialogObj)
            dialogArr.forEach(dialog => {
                this.dialogObj[dialog] = false
            })
            if (this.$refs.formTemplate) {
                this.templateInShow = this.$refs.formTemplate.generateFormTemplate()
            }
            if (this.$refs.btnRowTemplate) {
                this.templateInShow = this.$refs.btnRowTemplate.generateBtnRowTemplate()
            }
            this.templateShow = true
        },
        dialogShow(type, title) {
          this.dialogVisible = true
          this.dialogObj[type] = true
          this.templateShow = false
          this.dialogTitle = title
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
