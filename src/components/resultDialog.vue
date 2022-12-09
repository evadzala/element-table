<template>
  <el-dialog v-model="resultDialogVisible" title="報告資訊" @close="emit('closeDialogVisible')">
    <el-form :label-position="left">
      <el-form-item>
        {{ data[0].imageName }}
      </el-form-item>
      <el-form-item>
        上傳者：{{ data[0].author }}
      </el-form-item>
      <el-form-item>
        理賠案號：XXXXX
      </el-form-item>
      <el-form-item>
        AI版本：SSS
      </el-form-item>
      <el-form-item>
        推論時間：{{ eventTime }}
      </el-form-item>
      <el-form-item>
        結果：汲汲營營忘記身邊的人需要愛和關心　藉口總是拉遠了距離
不知不覺無聲無息　我們總是在抱怨事與願違　卻不願意回頭看看自己
想想自己　到底做了什麼蠢事情　也許是上帝給我一個試煉
只是這傷口需要花點時間　只是會想念過去的一切
那些人事物會離我遠去　而我們終究也會遠離變成回憶
      </el-form-item>
    </el-form>
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
    const eventTime = new Date()

    watch(() => props.isVisible, (value) => {
      resultDialogVisible.value = value
    })

    return {
      resultDialogVisible,
      eventTime,
      emit: context.emit,
    }
  }
}
</script>

<style>

</style>