<template>
  <div>
    <div>
      <div>表格数据模板</div>
      <div style="text-align: left">
        table:
      </div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="tableInShow">
      </el-input>
    </div>
    <div>
      <div>生成表格头部逻辑</div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="tableTemplateHead">
      </el-input>
    </div>
    <div>
      <div>生成表格各行逻辑</div>
      <div style="text-align: left">
        btnArr.forEach(btnType => {
      </div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="tableTemplateCyclicLogic">
      </el-input>
      <div style="text-align: left">
        })
      </div>
    </div>
    <div>
      <div>生成表格尾部逻辑</div>
      <el-input
        type="textarea"
        :rows="10"
        placeholder="请输入内容"
        v-model="tableTemplateTail">
      </el-input>
    </div>
  </div>
</template>

<script>
export default {
  name: 'tableTemplate',
  data() {
    return {
      tableInShow: '',
      table: {
        bindingPaging: true,
        model: 'tableData',
        pagingInFront: false,
        haveSelection: true,
        column: [
          {
            prop: 'menufacturer',
            name: '飞机厂商',
            sortable: true,
            template: '',
            formatter: 'formatterMen'
          },
          {
            prop: 'actLong',
            name: '长机型',
            sortable: true,
            template: '<el-button type="text" @click.stop="handleViewSigle(scope.row.id)">{{ scope.row.actLong }}</el-button>',
            formatter: null
          },
          {
            prop: 'actShort',
            name: '短机型',
            sortable: true,
            template: '',
            formatter: null
          },
          {
            prop: 'fleet',
            name: '机队',
            sortable: true,
            template: '',
            formatter: null
          },
          {
            prop: 'updateBy',
            name: '维护人',
            sortable: true,
            template: '',
            formatter: null
          },
          {
            prop: 'updateDate',
            name: '维护时间',
            sortable: true,
            template: '',
            formatter: 'formatterTime'
          }
        ]
      },
      templateMethods: {
        formatterTime: `(row, column, cellValue) => {
          if (cellValue) return parseTime(cellValue)
          return cellValue
        }`
      },
      tableTemplateHead: ``,
      tableTemplateCyclicLogic: ``,
      tableTemplateTail: ``
    }
  },
  mounted () {
    this.tableInShow = JSON.stringify(this.table, null, 2)
    this.generateTableTemplate()
  },
  methods: {
    generateTableTemplate() {
      this.table = JSON.parse(this.tableInShow)
      let str = ''
      if (this.table.bindingPaging === true) {
        if (this.table.haveSelection === true) {
          str += `<table-pagination ref="table" :tableData="${this.table.model}" :height="window.innerHeight - 320"
                service @service-fn="handleService" @row-click="rowClick" @selection-change="selectionChange"
                v-loading="listLoading">`
        } else {
          str += `<table-pagination ref="table" :tableData="${this.table.model}" :height="window.innerHeight - 320"
                service @service-fn="handleService" v-loading="listLoading">`
        }
        str += this.table.haveSelection === true ? `<el-table-column type="selection" width="43" />` : ``
        this.table.column.forEach(column => {
          if (column.template === '') {
            str += `<el-table-column prop="${column.prop}" label="${column.name}" align="center"`
            str += column.sortable === true ? ` sortable="custom" ` : ``
            str += column.formatter !== null ? ` :formatter="${column.formatter}" ` : ``
            str += `/>`
          } else {
            str += `<el-table-column label="${column.name}" align="center"`
            str += column.sortable === true ? ` sortable="custom" ` : ``
            str += column.formatter !== null ? ` :formatter="${column.formatter}" ` : ``
            str += `>`
            str += `<template slot-scope="scope">`
            str += column.template
            str += `</template>`
            str += `</el-table-column>`
          }
        })
        str += `</table-pagination>`
        console.log(str)
        return str
      }
    }
  }
}
</script>

<style scoped>

</style>