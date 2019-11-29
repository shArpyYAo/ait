<template>
  <div>
    固定内容
    <el-input style="width: 30vw"
              type="textarea"
              :rows="10"
              placeholder="请输入内容"
              v-model="dialogOuter">
    </el-input>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogOuter: `<el-dialog
      :title="dialog.title"
      :visible.sync="dialog.show"
      :close-on-click-modal="false"
      :close-on-press-escape="false"
      @close="handleClose"
    >
      <dialog-main
        ref="dialogMain"
        :visible="dialog.show"
        :type="dialog.type"
        :id="dialog.id"
        @success="handleSuccess"
      />
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="handleConfirm()" v-if="dialog.type !== 'view'" :disabled="confirmButton">确 定</el-button>
        <el-button @click="dialog.show = false">取 消</el-button>
      </div>
    </el-dialog>`
    }
  }
}
</script>

<style scoped>

</style>