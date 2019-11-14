<template>
  <div>
    <div>
      <div>按钮数据模板</div>
      <div style="text-align: left">
        btn:
      </div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="btnInShow">
      </el-input>
    </div>
    <div>
      <div>生成模板头部逻辑</div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="btnRowTemplateHead">
      </el-input>
    </div>
    <div>
      <div>生成模板各按钮逻辑</div>
      <div style="text-align: left">
        btnArr.forEach(btnType => {
      </div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="btnRowTemplateCyclicLogic">
      </el-input>
      <div style="text-align: left">
        })
      </div>
    </div>
    <div>
      <div>生成模板尾部逻辑</div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="btnRowTemplateTail">
      </el-input>
    </div>
  </div>
</template>

<script>
export default {
  name: 'btnRowTemplate',
  data() {
    return {
      btnInShow: '',
      btn: {
        add: {
          name: '$t(\'btn.add\')',
          clickMethod: 'handleDialog(\'add\')',
          type: 'green',
          icon: 'el-icon-fa fa-plus'
        },
        view: {
          name: '$t(\'btn.view\')',
          clickMethod: 'handleDialog(\'view\')',
          type: 'danger',
          icon: 'el-icon-fa fa-search-plus'
        },
        edit: {
          name: '$t(\'btn.edit\')',
          clickMethod: 'handleDialog(\'edit\')',
          type: 'info',
          icon: 'el-icon-fa fa-edit'
        },
        del: {
          name: '$t(\'btn.delete\')',
          clickMethod: 'handleDel()',
          type: 'warning',
          icon: 'el-icon-fa fa-trash'
        }
      },
      btnRowTemplateHead: `str += \`<div class="tool-bar">\`;return str`,
      btnRowTemplateCyclicLogic:
        `str += \`<el-button size="mini" type="\`;
         str += btn[btnType].type + \`" icon="\`;
         str += btn[btnType].icon + \`" @click="\`;
         str += btn[btnType].clickMethod + \`"\`;
         str += btnType === 'del' ?
          (\`style="float:right;">{{\` + btn[btnType].name + \`}}</el-button>\`) :
           (\`>{{\` + btn[btnType].name + \`}}</el-button>\`)
         return str`,
      btnRowTemplateTail: `str += \`</div>\`;return str`
    }
  },
  mounted () {
    this.btnInShow = JSON.stringify(this.btn, null, 2)
    this.generateBtnRowTemplate()
  },
  methods: {
    generateBtnRowTemplate() {
      this.btn = JSON.parse(this.btnInShow)
      let str = ''
      const headLogic = new Function('str', this.btnRowTemplateHead)
      str = headLogic(str)
      let btnArr = []
      btnArr = Object.keys(this.btn)
      let CyclicLogic = new Function('str', 'btnType', 'btn', this.btnRowTemplateCyclicLogic)
      let CyclicStr = ''
      btnArr.forEach(btnType => {
        CyclicStr = CyclicLogic(CyclicStr, btnType, this.btn)
      })
      str += CyclicStr
      const tailLogic = new Function('str', this.btnRowTemplateTail)
      str = tailLogic(str)
      return str
    }
  }
}
</script>

<style scoped>

</style>