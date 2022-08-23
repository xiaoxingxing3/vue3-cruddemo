<template>
  <div class="table-box">
    <!--标题-->
    <div class="title">
      <h2>最简单的CURD demo</h2>
    </div>
    <!--query-box区域-->
    <div class="query-box">
      <el-input v-model="queryInput" placeholder="请搜索姓名"/>
      <el-button type="primary">增加</el-button>
    </div>
    <div class="data-box">
      <!--  数据区域-->
      <el-table
          ref="multipleTableRef"
          :data="tableData"
          @selection-change="handleSelectionChange"
          border>
        <el-table-column type="selection" width="55" />
        <el-table-column fixed prop="date" label="Date" width="150"/>
        <el-table-column prop="name" label="Name" width="120"/>
        <el-table-column prop="state" label="State" width="120"/>
        <el-table-column prop="city" label="City" width="120"/>
        <el-table-column prop="address" label="Address" width="600"/>
        <el-table-column prop="zip" label="Zip" width="120"/>
        <el-table-column label="Operations" width="120">
          <template #default>
            <el-button link type="primary" size="small" @click="handleRowClick"
            >Detail
            </el-button
            >
            <el-button link type="primary" size="small">Edit</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>

</template>


<script setup>
import {ref} from 'vue'

/*数据*/
let queryInput = ref('')
let tableData = ref([
  {
    date: '2016-05-03',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
  },
  {
    date: '2016-05-04',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
  },
])
let multipleSelection = ref([])

/*方法*/
let handleRowClick = () => {
  console.log('click')
}
const handleSelectionChange = (val) => {
  multipleSelection.value = val
  console.log(val);
}

</script>

<style scoped>

.table-box {
  margin: 0 auto;
  width: 600px;
  text-align: center;
}

.query-box {
  display: flex;
  justify-content: space-between;
}

.data-box {
  margin-top: 20px;
}

.el-input {
  width: 200px;
}

</style>
