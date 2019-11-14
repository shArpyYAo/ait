<template>
  <div>
    <div>
      <el-radio-group v-model="templateRadio" @change="changeTemplate">
        <el-radio :label="1">简单模板</el-radio>
        <el-radio :label="2">隐藏查询项的模板</el-radio>
      </el-radio-group>
      <div>表单数据模板</div>
      <div style="text-align: left">
        formSearch:
      </div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="formSearchInShow">
      </el-input>
    </div>
    <div>
      <div>生成模板头部逻辑</div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="usingTemplateHead">
      </el-input>
    </div>
    <div>
      <div>生成各查询项逻辑</div>
      <div style="text-align: left">
        this.formSearch.data.forEach(obj => {
      </div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="usingTemplateCyclic">
      </el-input>
      <div style="text-align: left">
        })
      </div>
    </div>
    <div v-if="templateRadio === 2">
      <div>生成各隐藏查询项逻辑</div>
      <div style="text-align: left">
        this.formSearch.hidingData.forEach(obj => {
      </div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="formTemplateHidingCyclicLogic">
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
        v-model="usingTemplateFoot">
      </el-input>
    </div>
  </div>
</template>

<script>
export default {
  name: 'formTemplate',
  data() {
    return {
      templateRadio: 2,
      usingForm: {},
      formSearch: {
        option: {
          paging: true
        },
        data: [
          {
            name: '飞机厂商',
            type: 'date',
            model: 'menufacturer',
            dataName: 'menufacturer',
            isHiding: false,
            data: [
              {
                id: 1,
                code: 1,
                name: '波音'
              }
            ]
          },
          {
            name: '长机型',
            type: 'input',
            model: 'actLong',
            isHiding: false,
          },
          {
            name: '短机型',
            type: 'input',
            model: 'actShort',
            isHiding: true,
          },
          {
            name: '机队',
            type: 'select',
            model: 'fleet',
            dataName: 'fleet',
            isHiding: true,
            data: [
              {
                id: 1,
                code: 1,
                name: 'A330'
              }
            ]
          }
        ]
      },
      formSearch1: {
        option: {
          paging: true
        },
        showingData: [
          {
            name: '起始日期',
            type: 'daterange',
            model: 'formData.data.dateString',
            dataName: 'formData.data.dateString'
          },
          {
            name: '处理日期',
            type: 'daterange',
            model: 'formData.data.handleDateString',
            dataName: 'formData.data.handleDateString'
          },
          {
            name: '短机型',
            type: 'input',
            model: 'actShort'
          },
          {
            name: '机队',
            type: 'select',
            model: 'fleet',
            dataName: 'fleet'
          }
        ],
        hidingData: [
          {
            name: '短机型',
            type: 'input',
            model: 'actShort'
          },
          {
            name: '机队',
            type: 'select',
            model: 'fleet',
            dataName: 'fleet'
          }
        ]
      },
      formSearchInShow: '',
      formTemplateHead: `str = formSearch.option.paging === true ?
        \`<el-form id="id" ref="searchForm" :model="formData.data" v-loading="formLoading" class="search-form">\` :
        \`<el-form id="id" ref="searchForm" :model="formData" v-loading="formLoading" class="search-form">\`;
        str += \`<div class="search-form-box">\`; return str`,
      formTemplateCyclicLogic:
        `str += \`<el-form-item prop="\`;
         str += obj.model + '">';
         str += \`<span>\`;
         str += obj.name;
         str += \`：</span>\`;
         if (obj.type === 'input') {
           str += \`<el-input size="small" v-model="formData.data.\`;
           str += obj.model;str += \`" :placeholder="$t('text.enter')" />\`;
         }
         if (obj.type === 'select') {
           str += \`<el-select size="small" v-model="formData.data.\`;
           str += obj.model;str += \`" :placeholder="$t('text.select')">\`;
           str += \`<el-option value="" :label="$t('text.all')" />\`;
           str += \`<el-option v-for="option in \`;
           str += obj.dataName;str += \`" :key="option.id" :value="option.code" :label="option.name"/>\`;
           str += \`</el-select>\`;
         }
         str += \`</el-form-item>\`;
         return str`,
      formTemplateFootLogic: `return \`<el-form-item>
        <el-button type="primary" size="small" icon="el-icon-fa fa-search" @click="handleSearch()" :disabled="listLoading || !formInited">{{ $t('btn.search') }}</el-button>
        <el-button type="info" size="small" icon="el-icon-fa fa-refresh" @click="handleReset">{{ $t('btn.reset') }}</el-button>
        </el-form-item>
        </div>
        </el-form>\``,
      formTemplateHead1:
        `str = \`<el-form class="search-form" label-width="100px" ref="searchForm"
          style="flex-wrap: wrap" v-loading="formLoading" :model="formData.data">
          <div style="width: 100%">
            <div style="display: flex;flex-wrap: wrap">\`;
        return str`, // 有[更多查询项]按钮的模板
      formTemplateCyclicLogic1:
        `str += \`<el-form-item prop="\`;
         str += obj.model + '" '
         str += 'label="' + obj.name + '：">'`,
      formTemplateHidingCyclicLogic: ``,
      formTemplateFootLogic1: ``,
      usingTemplateHead: ``,
      usingTemplateCyclic: ``,
      usingTemplateFoot: ``
    }
  },
  mounted() {
    this.changeTemplate()
    this.formSearchInShow = JSON.stringify(this.usingForm, null, 2)
    this.generateFormTemplate()
  },
  methods: {
    changeTemplate() {
      this.usingTemplateHead = this.templateRadio === 2 ? this.formTemplateHead1 : this.formTemplateHead
      this.usingTemplateCyclic = this.templateRadio === 2 ? this.formTemplateCyclicLogic1 : this.formTemplateCyclicLogic
      this.usingTemplateFoot = this.templateRadio === 2 ? this.formTemplateFootLogic1 : this.formTemplateFootLogic
      this.usingForm = this.templateRadio === 2 ? this.formSearch1 : this.formSearch
      this.formSearchInShow = JSON.stringify(this.usingForm, null, 2)
    },
    generateFormTemplate() {
      this.usingForm = JSON.parse(this.formSearchInShow)
      let str = ''
      const headLogic = new Function('str', 'formSearch', this.usingTemplateHead)
      str = headLogic(str, this.usingForm)
      let CyclicLogic = new Function('str', 'obj', this.usingTemplateCyclic)
      let CyclicStr = ''
      this.usingForm.data && this.usingForm.data.forEach(obj => {
        CyclicStr = CyclicLogic(CyclicStr, obj)
      })
      this.usingForm.showingData && this.usingForm.showingData.forEach(obj => {
        CyclicStr = CyclicLogic(CyclicStr, obj)
      })
      this.usingTemplateCyclic = this.formTemplateHidingCyclicLogic
      CyclicLogic = new Function('str', 'obj', this.usingTemplateCyclic)
      this.usingForm.hidingData && this.usingForm.hidingData.forEach(obj => {
        CyclicStr = CyclicLogic(CyclicStr, obj)
      })
      str += CyclicStr
      let footLogic = new Function(this.usingTemplateFoot)
      str += footLogic()
      return str
    }
  }
}
</script>

<style scoped>

</style>