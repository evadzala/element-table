<template>
  <el-dialog v-model="resultDialogVisible">
    <div>{{ data }}</div>
  </el-dialog>
</template>

<script>
import { ref, watch } from 'vue'

export default {
  props: {
    isVisible: {
      type: Boolean
    },
    data: {
      type: Array,
    }
  },
  setup (props) {
    const resultDialogVisible = ref(false)

    watch(() => props.isVisible, (value) => {
      resultDialogVisible.value = value
    })

    return {
      resultDialogVisible,
    }
  }
}
</script>

<style>

</style>