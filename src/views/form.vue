<template>
  <el-container>
    <el-main>
      <div>
        <h1>醫療影像與AI推論</h1>
        <div class="sub_title">
          <div>案件編號: {{ eventNumber }}</div>
          <el-button type="primary" @click="sentData" :disabled="!(multipleEvent.length > 0)">進行AI推論</el-button>
        </div>
      </div>
      <div class="table_border">
        <el-table
          :data="pageData"
          style="width: 100%"
          @selection-change="handleSelectionChange"
        >
          <el-table-column type="selection" />
          <el-table-column property="imageName" label="影像檔名" />
          <el-table-column property="author" label="上傳者" />
          <el-table-column property="bodyPartCheck" label="身體部位" />
          <el-table-column property="tool" label="診斷工具" />
          <el-table-column property="AIModel" label="AI推論模組">
            <template #default="scope">
              <el-select v-model="scope.row.AIModel" class="m-2" placeholder="Select" size="small">
                <el-option
                  v-for="item in modelOptions"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                />
              </el-select>
            </template>
          </el-table-column>
          <el-table-column property="done" label="推論進度" :formatter="scheduleFormatter" />
    
        </el-table>
        <div class="proportion">
          <div>
            已推論 {{ hasDone() }} / 總張數{{ homeworkData.length }}
          </div>
        </div>
      </div>
      <div class="pagination">
        <el-pagination
          background
          layout="prev, pager, next"
          :page-size="10"
          :total="homeworkData.length"
          :default-current-page="currentPage"
          @update:current-page="pageChange"
        />
      </div>
    </el-main>
    <result-dialog :isVisible="resultDialogVisible" :data="beSelectedData" @closeDialogVisible="closeDialog" />
  </el-container>
</template>

<script>
import { ref } from '@vue/reactivity'
import resultDialog from '../components/resultDialog.vue'

export default {
  components: {
    resultDialog,
  },
  setup () {
    const eventNumber = 1423
    const multipleEvent = ref([])
    const currentPage = ref(1)
    const beSelectedData = ref([])
    const resultDialogVisible = ref(false)
    const modelOptions = [
      {
        value: 1,
        label: '骨折'
      },
      {
        value: 2,
        label: '外傷'
      },
      {
        value: 3,
        label: '內傷'
      },
    ]
    let pageData = ref([])
    let homeworkData = [
      {
        imageName: 'Ser1-Img1',
        author: 'AAA',
        bodyPartCheck: '骨盆',
        tool: 'CR',
        AIModel: 1,
        done: true
      }
    ]
    const makeHomeworkData = () => {
      const amount = 25
      for (let i = 2; i < amount; i++) {
        homeworkData.push({
          imageName: `Ser${i}-Img${i}`,
          author: 'AAA',
          bodyPartCheck: '骨盆',
          tool: 'CR',
          AIModel: 1,
          done: true
        })
      }

      for (let i = 25; i < amount * 2 + 3; i++) {
        homeworkData.push({
          imageName: `Ser${i}-Img${i}`,
          author: 'AAA',
          bodyPartCheck: '骨盆',
          tool: 'CR',
          AIModel: 1,
          done: false
        })
      }
    }
    makeHomeworkData()

    function defaultPageData () {
      pageData.value = homeworkData.slice(0,10)
    }
    defaultPageData()

    const handleSelectionChange = (value) => {
      multipleEvent.value = value
    }

    function sentData () {
      beSelectedData.value = multipleEvent.value
      console.log('sentData', beSelectedData.value)
      resultDialogVisible.value = true
    }

    function closeDialog () {
      resultDialogVisible.value = false
    }

    const hasDone = () => {
      return homeworkData.filter(node => JSON.stringify(node.done) === 'true').length
    }

    function pageChange (val) {
      currentPage.value = val
      pageData.value = homeworkData.slice((val - 1) * 10, (val * 10))
    }

    function scheduleFormatter (row, col) {
      return row.done ? '完成推論' : '未完成推論'
    }

    return {
      eventNumber,
      currentPage,
      pageData,
      multipleEvent,
      modelOptions,
      handleSelectionChange,
      homeworkData,
      hasDone,
      pageChange,
      resultDialogVisible,
      sentData,
      closeDialog,
      beSelectedData,
      scheduleFormatter,
    }
  },
}
</script>

<style>
.sub_title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.table_border {
  border: 2px solid #909399;
  margin-bottom: 20px;
  padding: 15px;
}

.proportion {
  display: flex;
  justify-content: flex-start;
  margin: 20px 0;
}

.pagination {
  display: flex;
  justify-content: center;
}
</style>