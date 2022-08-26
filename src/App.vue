<template>
  <div class="table-box">
    <!--标题-->
    <div class="title">
      <h2>最简单的CURD demo</h2>
    </div>
    <!--query-box区域-->
    <div class="query-box">
      <el-input v-model="queryInput" class="queryinput" placeholder="请搜索姓名"/>
      <div class="but-list">
        <el-button type="primary" @click="handleAdd">增加</el-button>
        <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length>0">删除多选</el-button>
      </div>
    </div>
    <div class="data-box">
      <!--  数据区域-->
      <el-table
          ref="multipleTableRef"
          :data="tableData"
          @selection-change="handleSelectionChange"
          border>
        <el-table-column fixed type="selection" width="55" />
        <el-table-column prop="name" label="姓名" width="80"/>
        <el-table-column prop="tel" label="电话" width="120"/>
        <el-table-column prop="email" label="邮箱" width="120"/>
        <el-table-column prop="state" label="状态" width="120"/>
        <el-table-column prop="address" label="地址" width="300"/>
        <el-table-column fixed="right" label="操作" width="120">
          <template #default="scope">
            <el-button link type="primary" size="small" @click="handleRowDel(scope.row)" style="color:#F56C6C">删除
            </el-button>
            <el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
<!--弹窗-->
    <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add'? '新增':'编辑'">
      <el-form :model="tableForm">
        <el-form-item label="姓名" :label-width="formLabelWidth">
          <el-input v-model="tableForm.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="电话" :label-width="formLabelWidth">
          <el-input v-model="tableForm.tel" autocomplete="off" />
        </el-form-item>
        <el-form-item label="邮箱" :label-width="formLabelWidth">
          <el-input v-model="tableForm.email" autocomplete="off" />
        </el-form-item>
        <el-form-item label="状态" :label-width="formLabelWidth">
          <el-input v-model="tableForm.state" autocomplete="off" />
        </el-form-item>
        <el-form-item label="地址" :label-width="formLabelWidth">
          <el-input v-model="tableForm.address" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogconfirm">确认</el-button>
      </span>
      </template>
    </el-dialog>
  </div>
</template>


<script setup>
import {ref} from 'vue'

/*数据*/
let queryInput = $ref('')
let tableData = $ref( [
  {
    id:"1",
    name: 'Tom1',
    tel: '13672863701',
    email:"1123@qq.com",
    state: '离职',
    address: 'No. 189, Grove St, Los Angeles'
  },
  {
    id:"2",
    name: 'Tom2',
    tel: '13672863702',
    email:"1123@qq.com",
    state: '在职',
    address: 'No. 189, Grove St, Los Angeles'
  },
  {
    id:"3",
    name: 'Tom3',
    tel: '13672863703',
    email:"1123@qq.com",
    state: '离职',
    address: 'No. 189, Grove St, Los Angeles'
  },
])
let multipleSelection = $ref([])
let dialogFormVisible = $ref(false)
let tableForm = $ref({
  name:"张三",
  email:"1123@qq.com",
  tel:"12345678910",
  state:"在职",
  address:"江苏南京"
})
let dialogType = $ref('edit')

/*方法*/
//删除一条
const handleRowDel = ({id}) => {
  //1.通过id 获取到当前数据的索引值
  let index = tableData.findIndex(item=>item.id===id)

  //2.删除当前index值对应在tableData中的数据
  tableData.splice(index, 1)

}
//删除多选
const handleDelList = ()=>{
  multipleSelection.forEach(id=>{
    handleRowDel({id})
  })
  multipleSelection = []
}

//编辑
const handleEdit = (row)=>{
  dialogFormVisible = true
  dialogType = 'edit'
  tableForm = row
}

//选中
const handleSelectionChange = (val) => {
  // multipleSelection = val
  // console.log(val);
  //先清空之前选中的
  multipleSelection = []
  //然后再将id push进去
  val.forEach(item=>{
    multipleSelection.push(item.id)
  })
  // console.log(multipleSelection);
}
//新增
const handleAdd = () =>{
  dialogFormVisible = true
  tableForm = {}
  dialogType = 'add'
}
//确认
const dialogconfirm = ()=>{
  dialogFormVisible = false

  // 判断是新增还是编辑
  if(dialogType === 'add'){
    //1.获取数据
    //2.添加到tableData
    tableData.push({
          id: (tableData.length + 1).toString(),
          ...tableForm,
        }
    )
  }else {
    //1. 获取当前数据的索引值
    let index = tableData.findIndex(item=>item.id === tableForm.id)

    //2. 替换当前索引值的数据
    tableData[index] = tableForm
  }



}


</script>

<style scoped>

.table-box {
  margin: 200px auto;
  width: 600px;
  text-align: center;
}

.query-box {
  display: flex;
  justify-content: space-between;
}
.queryinput{
  width: 200px;
}

.data-box {
  margin-top: 20px;
}

</style>
