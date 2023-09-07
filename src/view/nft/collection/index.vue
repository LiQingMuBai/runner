<template>
  <div>
    <div class="gva-search-box">
      <el-form ref="searchForm" :inline="true" :model="searchInfo">
        <el-form-item label="一级渠道">
          <el-input v-model="searchInfo.primaryChannel" placeholder="" />
        </el-form-item>
        <el-form-item label="授权地址">
          <el-input v-model="searchInfo.approvalAddress" placeholder="" />
        </el-form-item>
<!--        <el-form-item label="地址">-->
<!--          <el-input v-model="searchInfo.apiGroup" placeholder="" />-->
<!--        </el-form-item>-->
<!--        <el-form-item label="渠道">-->
<!--          <el-select v-model="searchInfo.method" clearable placeholder="请选择">-->
<!--            <el-option-->
<!--                v-for="item in methodOptions"-->
<!--                :key="item.value"-->
<!--                :label="`${item.label}(${item.value})`"-->
<!--                :value="item.value"-->
<!--            />-->
<!--          </el-select>-->
<!--        </el-form-item>-->
        <el-form-item>
          <el-button type="primary" icon="search" @click="onSubmit">查询</el-button>
          <el-button icon="refresh" @click="onReset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="gva-table-box">
      <div class="gva-btn-list">
<!--        <el-button type="primary" icon="plus" @click="openDialog('addApi')">新增</el-button>-->
      </div>
      <el-table :data="tableData" @sort-change="sortChange" @selection-change="handleSelectionChange">
        <el-table-column
            type="selection"
            width="55"
        />
<!--        <el-table-column align="left" label="id" min-width="60" prop="ID" sortable="custom" />-->
<!--        <el-table-column align="left" label="客户地址" min-width="350" prop="customerAddress" sortable="custom" />-->
<!--        <el-table-column align="left" label="网络" min-width="80" prop="network" sortable="custom" />-->
<!--        <el-table-column align="left" label="授权地址" min-width="350" prop="approvalAddress" sortable="custom" />-->
<!--        <el-table-column align="left" label="余额" min-width="80" prop="balance" sortable="custom" />-->
<!--        <el-table-column align="left" label="币种" min-width="80" prop="token" sortable="custom" />-->
        <el-table-column align="left" label="一级渠道" min-width="150" prop="primaryChannel" sortable="custom"/>
        <el-table-column align="left" label="授权地址" min-width="350" prop="approvalAddress" sortable="custom" />
        <el-table-column align="left" label="已提取" min-width="100" prop="withdrawAmount" sortable="custom" >
<!--        <el-table-column align="left" label="备注" min-width="150" prop="desc" sortable="custom" />-->

<!--        <el-table-column align="left" label="二级渠道" min-width="150" prop="secondaryChannel" sortable="custom" >-->
          <!--&lt;!&ndash;        <el-table-column align="left" label="一级渠道" min-width="150" prop="description" sortable="custom" />&ndash;&gt;-->

          <!--          <template #default="scope">-->
          <!--            <div>-->
          <!--              {{ scope.row.method }} / {{ methodFilter(scope.row.method) }}-->
          <!--            </div>-->
          <!--          </template>-->
        </el-table-column>

        <!--
        <el-table-column align="left" fixed="right" label="操作" width="300">
          <template #default="scope">
            <el-button
                icon="edit"

                type="primary"
                link
                @click="editApiFunc(scope.row)"
            >编辑</el-button>
            <el-button
                icon="delete"

                type="primary"
                link
                @click="deleteApiFunc(scope.row)"
            >刷新</el-button>

            <el-button
                icon="edit"

                type="primary"
                link
                @click="editApiFunc(scope.row)"
            >提现</el-button>

          </template>
        </el-table-column>
        -->
      </el-table>
      <div class="gva-pagination">
        <el-pagination
            :current-page="page"
            :page-size="pageSize"
            :page-sizes="[10, 30, 50, 100]"
            :total="total"
            layout="total, sizes, prev, pager, next, jumper"
            @current-change="handleCurrentChange"
            @size-change="handleSizeChange"
        />
      </div>

    </div>

    <el-dialog v-model="dialogFormVisible" :before-close="closeDialog" :title="dialogTitle">
      <!--      <warning-bar title="新增API，需要在角色管理内配置权限才可使用" />-->
      <el-form ref="apiForm" :model="form" :rules="rules" label-width="80px">
        <el-form-item label="用户地址" prop="customerAddress">
          <el-input v-model="form.customerAddress" autocomplete="off" />
        </el-form-item>
        <el-form-item label="授权地址" prop="approvalAddress">
          <el-input v-model="form.approvalAddress" autocomplete="off" />
        </el-form-item>

        <!--        <el-form-item label="地址类型" prop="network">-->
        <!--          <el-input v-model="form.network" autocomplete="off" />-->
        <!--        </el-form-item>-->

        <el-form-item label="地址类型">
          <el-select v-model="form.network" clearable placeholder="请选择">
            <el-option
                v-for="item in networkOptions"
                :key="item.value"
                :label="`${item.label}(${item.value})`"
                :value="item.value"
            />
          </el-select>
        </el-form-item>

        <el-form-item label="币种类型">
          <el-select v-model="form.token" clearable placeholder="请选择">
            <el-option
                v-for="item in tokenOptions"
                :key="item.value"
                :label="`${item.label}(${item.value})`"
                :value="item.value"
            />
          </el-select>
        </el-form-item>

        <!--        <el-form-item label="" prop="token">-->
        <!--          <el-input v-model="form.token" autocomplete="off" />-->
        <!--        </el-form-item>-->


        <el-form-item label="备注信息" prop="desc">
          <el-input v-model="form.desc" autocomplete="off" />
        </el-form-item>

      </el-form>
      <template #footer>
        <div class="dialog-footer">
          <el-button @click="closeDialog">取 消</el-button>
          <el-button type="primary" @click="enterDialog">确 定</el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'Api',
}
</script>

<script setup>
import {
  getApiById,
  getApiList,
  createApi,
  updateApi,
  deleteApi,
  deleteApisByIds,
  freshCasbin
} from '@/api/collection'
import { toSQLLine } from '@/utils/stringFun'
import WarningBar from '@/components/warningBar/warningBar.vue'
import { ref } from 'vue'
import { ElMessage, ElMessageBox } from 'element-plus'
import {QuestionFilled, VideoCameraFilled} from "@element-plus/icons-vue";
import { toDoc } from '@/utils/doc'

const methodFilter = (value) => {
  const target = methodOptions.value.filter(item => item.value === value)[0]
  return target && `${target.label}`
}

const apis = ref([])
const form = ref({
  path: '',
  apiGroup: '',
  method: '',
  description: ''
})

const tokenOptions = ref([
  {
    value: 'USDT',
    label: 'USDT',
    type: 'success'
  },
  {
    value: 'USDC',
    label: 'USDC',
    type: ''
  }
])

const networkOptions = ref([
  {
    value: 'ETH',
    label: '以太坊',
    type: 'success'
  },
  {
    value: 'TRX',
    label: '波场',
    type: 'warning'
  }
])

const type = ref('')
const rules = ref({
  path: [{ required: true, message: '请输入api路径', trigger: 'blur' }],
  apiGroup: [
    { required: true, message: '请输入组名称', trigger: 'blur' }
  ],
  method: [
    { required: true, message: '请选择请求方式', trigger: 'blur' }
  ],
  description: [
    { required: true, message: '请输入api介绍', trigger: 'blur' }
  ]
})

const page = ref(1)
const total = ref(0)
const pageSize = ref(10)
const tableData = ref([])
const searchInfo = ref({})

const onReset = () => {
  searchInfo.value = {}
}
// 搜索

const onSubmit = () => {
  page.value = 1
  pageSize.value = 10
  getTableData()
}

// 分页
const handleSizeChange = (val) => {
  pageSize.value = val
  getTableData()
}

const handleCurrentChange = (val) => {
  page.value = val
  getTableData()
}

// 排序
const sortChange = ({ prop, order }) => {
  if (prop) {
    if (prop === 'ID') {
      prop = 'id'
    }
    searchInfo.value.orderKey = toSQLLine(prop)
    searchInfo.value.desc = order === 'descending'
  }
  getTableData()
}

// 查询
const getTableData = async() => {
  const table = await getApiList({ page: page.value, pageSize: pageSize.value, ...searchInfo.value })
  if (table.code === 0) {
    tableData.value = table.data.list
    total.value = table.data.total
    page.value = table.data.page
    pageSize.value = table.data.pageSize
  }
}

getTableData()

// 批量操作
const handleSelectionChange = (val) => {
  apis.value = val
}

const deleteVisible = ref(false)
const onDelete = async() => {
  const ids = apis.value.map(item => item.ID)
  const res = await deleteApisByIds({ ids })
  if (res.code === 0) {
    ElMessage({
      type: 'success',
      message: res.msg
    })
    if (tableData.value.length === ids.length && page.value > 1) {
      page.value--
    }
    deleteVisible.value = false
    getTableData()
  }
}
const freshVisible = ref(false)
const onFresh = async() => {
  const res = await freshCasbin()
  if (res.code === 0) {
    ElMessage({
      type: 'success',
      message: res.msg
    })
  }
  freshVisible.value = false
}

// 弹窗相关
const apiForm = ref(null)
const initForm = () => {
  apiForm.value.resetFields()
  form.value = {
    path: '',
    apiGroup: '',
    method: '',
    description: ''
  }
}

const dialogTitle = ref('新增用户')
const dialogFormVisible = ref(false)
const openDialog = (key) => {
  switch (key) {
    case 'addApi':
      dialogTitle.value = '新增用户'
      break
    case 'edit':
      dialogTitle.value = '编辑用户'
      break
    default:
      break
  }
  type.value = key
  dialogFormVisible.value = true
}
const closeDialog = () => {
  initForm()
  dialogFormVisible.value = false
}

const editApiFunc = async(row) => {
  const res = await getApiById({ id: row.ID })
  form.value = res.data.api
  openDialog('edit')
}

const enterDialog = async() => {
  apiForm.value.validate(async valid => {
    if (valid) {
      switch (type.value) {
        case 'addApi':
        {
          const res = await createApi(form.value)
          if (res.code === 0) {
            ElMessage({
              type: 'success',
              message: '添加成功',
              showClose: true
            })
          }
          getTableData()
          closeDialog()
        }

          break
        case 'edit':
        {
          const res = await updateApi(form.value)
          if (res.code === 0) {
            ElMessage({
              type: 'success',
              message: '编辑成功',
              showClose: true
            })
          }
          getTableData()
          closeDialog()
        }
          break
        default:
          // eslint-disable-next-line no-lone-blocks
        {
          ElMessage({
            type: 'error',
            message: '未知操作',
            showClose: true
          })
        }
          break
      }
    }
  })
}

const deleteApiFunc = async(row) => {
  ElMessageBox.confirm('此操作将永久删除所有角色下该api, 是否继续?', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  })
      .then(async() => {
        const res = await deleteApi(row)
        if (res.code === 0) {
          ElMessage({
            type: 'success',
            message: '删除成功!'
          })
          if (tableData.value.length === 1 && page.value > 1) {
            page.value--
          }
          getTableData()
        }
      })
}

</script>

<style scoped lang="scss">
.warning {
  color: #dc143c;
}
</style>
