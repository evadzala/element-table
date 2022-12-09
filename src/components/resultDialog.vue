<template>
  <el-dialog v-model="resultDialogVisible" @close="emit('closeDialogVisible')">
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
  emit: ['closeDialogVisible'],
  setup (props, context) {
    const resultDialogVisible = ref(false)

    watch(() => props.isVisible, (value) => {
      resultDialogVisible.value = value
    })

    return {
      resultDialogVisible,
      emit: context.emit,
    }
  }
}
</script>

<style>

</style>