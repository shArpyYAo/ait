<template>
  <div>
    <div>模板展示</div>
    <el-input
      type="textarea"
      :rows="10"
      placeholder="请输入内容"
      v-model="templateInShow">
    </el-input>
  </div>
</template>

<script>
export default {
  name: 'showTemplate',
  props: {
    templateInShow: String
  },
}
</script>

<style scoped>

</style>