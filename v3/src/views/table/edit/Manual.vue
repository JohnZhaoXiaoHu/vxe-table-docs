<template>
  <div>
    <p class="tip">设置 <table-api-link prop="edit-config"/>={trigger: 'manual', mode: 'row'} 启用行编辑的功能，还可以配合 <table-api-link prop="revertData"/> 函数实现取消就还原数据<br></p>

    <vxe-table
      border
      resizable
      show-overflow
      ref="xTable"
      :loading="loading"
      :data="tableData"
      :edit-config="{trigger: 'manual', mode: 'row'}">
      <vxe-column type="seq" width="60"></vxe-column>
      <vxe-column field="name" title="Name" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.name" type="text"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="role" title="Role" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.role" type="text" placeholder="请输入昵称"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="sex" title="Sex" :edit-render="{}">
        <template #default="{ row }">
          <span>{{ formatSex(row.sex) }}</span>
        </template>
        <template #edit="{ row }">
          <vxe-select v-model="row.sex" transfer>
            <vxe-option v-for="item in sexList" :key="item.value" :value="item.value" :label="item.label"></vxe-option>
          </vxe-select>
        </template>
      </vxe-column>
      <vxe-column field="sex2" title="多选下拉" :edit-render="{}">
        <template #default="{ row }">
          <span>{{ formatMultiSex(row.sex2) }}</span>
        </template>
        <template #edit="{ row }">
          <vxe-select v-model="row.sex2" multiple transfer>
            <vxe-option v-for="item in sexList" :key="item.value" :value="item.value" :label="item.label"></vxe-option>
          </vxe-select>
        </template>
      </vxe-column>
      <vxe-column field="num6" title="Number" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.num6" type="number" placeholder="请输入数值"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="date12" title="Date" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.date12" type="date" placeholder="请选择日期" transfer></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="date13" title="Week" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.date13" type="week" placeholder="请选择日期" transfer></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="address" title="Address" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.address" type="text"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column title="操作" width="160">
        <template #default="{ row }">
          <template v-if="$refs.xTable.isActiveByRow(row)">
            <vxe-button @click="saveRowEvent(row)">保存</vxe-button>
            <vxe-button @click="cancelRowEvent(row)">取消</vxe-button>
          </template>
          <template v-else>
            <vxe-button @click="editRowEvent(row)">编辑</vxe-button>
          </template>
        </template>
      </vxe-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="xml">{{ demoCodes[0] }}</pre-code>
      <pre-code class="javascript">{{ demoCodes[1] }}</pre-code>
    </pre>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loading: false,
      tableData: [
        { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: '0', sex2: ['0'], num1: 40, age: 28, address: 'Shenzhen', date12: '', date13: '' },
        { id: 10002, name: 'Test2', nickname: 'T2', role: 'Designer', sex: '1', sex2: ['0', '1'], num1: 44, age: 22, address: 'Guangzhou', date12: '', date13: '2020-08-20' },
        { id: 10003, name: 'Test3', nickname: 'T3', role: 'Test', sex: '0', sex2: ['1'], num1: 200, age: 32, address: 'Shanghai', date12: '2020-09-10', date13: '' },
        { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: '1', sex2: ['1'], num1: 30, age: 23, address: 'Shenzhen', date12: '', date13: '2020-12-04' },
        { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: '0', sex2: ['1', '0'], num1: 20, age: 30, address: 'Shanghai', date12: '2020-09-20', date13: '' },
        { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: '1', sex2: ['0'], num1: 10, age: 21, address: 'Shenzhen', date12: '', date13: '' },
        { id: 10007, name: 'Test7', nickname: 'T7', role: 'Develop', sex: '0', sex2: ['0'], num1: 5, age: 29, address: 'Shenzhen', date12: '2020-01-02', date13: '2020-09-20' },
        { id: 10008, name: 'Test8', nickname: 'T8', role: 'PM', sex: '1', sex2: ['0'], num1: 2, age: 35, address: 'Shenzhen', date12: '', date13: '' }
      ],
      sexList: [
        { label: '', value: '' },
        { label: '男', value: '1' },
        { label: '女', value: '0' }
      ],
      demoCodes: [
        `
        <vxe-table
          border
          resizable
          show-overflow
          ref="xTable"
          :loading="loading"
          :data="tableData"
          :edit-config="{trigger: 'manual', mode: 'row'}">
          <vxe-column type="seq" width="60"></vxe-column>
          <vxe-column field="name" title="Name" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.name" type="text"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="role" title="Role" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.role" type="text" placeholder="请输入昵称"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="sex" title="Sex" :edit-render="{}">
            <template #default="{ row }">
              <span>{{ formatSex(row.sex) }}</span>
            </template>
            <template #edit="{ row }">
              <vxe-select v-model="row.sex" transfer>
                <vxe-option v-for="item in sexList" :key="item.value" :value="item.value" :label="item.label"></vxe-option>
              </vxe-select>
            </template>
          </vxe-column>
          <vxe-column field="sex2" title="多选下拉" :edit-render="{}">
            <template #default="{ row }">
              <span>{{ formatMultiSex(row.sex2) }}</span>
            </template>
            <template #edit="{ row }">
              <vxe-select v-model="row.sex2" multiple transfer>
                <vxe-option v-for="item in sexList" :key="item.value" :value="item.value" :label="item.label"></vxe-option>
              </vxe-select>
            </template>
          </vxe-column>
          <vxe-column field="num6" title="Number" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.num6" type="number" placeholder="请输入数值"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="date12" title="Date" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.date12" type="date" placeholder="请选择日期" transfer></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="date13" title="Week" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.date13" type="week" placeholder="请选择日期" transfer></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="address" title="Address" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.address" type="text"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column title="操作" width="160">
            <template #default="{ row }">
              <template v-if="$refs.xTable.isActiveByRow(row)">
                <vxe-button @click="saveRowEvent(row)">保存</vxe-button>
                <vxe-button @click="cancelRowEvent(row)">取消</vxe-button>
              </template>
              <template v-else>
                <vxe-button @click="editRowEvent(row)">编辑</vxe-button>
              </template>
            </template>
          </vxe-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              loading: false,
              tableData: [
                { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: '0', sex2: ['0'], num1: 40, age: 28, address: 'Shenzhen', date12: '', date13: '' },
                { id: 10002, name: 'Test2', nickname: 'T2', role: 'Designer', sex: '1', sex2: ['0', '1'], num1: 44, age: 22, address: 'Guangzhou', date12: '', date13: '2020-08-20' },
                { id: 10003, name: 'Test3', nickname: 'T3', role: 'Test', sex: '0', sex2: ['1'], num1: 200, age: 32, address: 'Shanghai', date12: '2020-09-10', date13: '' },
                { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: '1', sex2: ['1'], num1: 30, age: 23, address: 'Shenzhen', date12: '', date13: '2020-12-04' },
                { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: '0', sex2: ['1', '0'], num1: 20, age: 30, address: 'Shanghai', date12: '2020-09-20', date13: '' },
                { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: '1', sex2: ['0'], num1: 10, age: 21, address: 'Shenzhen', date12: '', date13: '' },
                { id: 10007, name: 'Test7', nickname: 'T7', role: 'Develop', sex: '0', sex2: ['0'], num1: 5, age: 29, address: 'Shenzhen', date12: '2020-01-02', date13: '2020-09-20' },
                { id: 10008, name: 'Test8', nickname: 'T8', role: 'PM', sex: '1', sex2: ['0'], num1: 2, age: 35, address: 'Shenzhen', date12: '', date13: '' }
              ],
              sexList: [
                { label: '', value: '' },
                { label: '男', value: '1' },
                { label: '女', value: '0' }
              ]
            }
          },
          methods: {
            formatSex (value) {
              if (value === '1') {
                return '男'
              }
              if (value === '0') {
                return '女'
              }
              return ''
            },
            formatMultiSex (values) {
              if (values) {
                return values.map(val => this.formatSex(val)).join(',')
              }
              return ''
            },
            editRowEvent (row) {
              const $table = this.$refs.xTable
              $table.setActiveRow(row)
            },
            saveRowEvent () {
              const $table = this.$refs.xTable
              $table.clearActived().then(() => {
                this.loading = true
                setTimeout(() => {
                  this.loading = false
                  this.$XModal.message({ content: '保存成功！', status: 'success' })
                }, 300)
              })
            },
            cancelRowEvent (row) {
              const $table = this.$refs.xTable
              $table.clearActived().then(() => {
                // 还原行数据
                $table.revertData(row)
              })
            }
          }
        }
        `
      ]
    }
  },
  methods: {
    formatSex (value) {
      if (value === '1') {
        return '男'
      }
      if (value === '0') {
        return '女'
      }
      return ''
    },
    formatMultiSex (values) {
      if (values) {
        return values.map(val => this.formatSex(val)).join(',')
      }
      return ''
    },
    editRowEvent (row) {
      const $table = this.$refs.xTable
      $table.setActiveRow(row)
    },
    saveRowEvent () {
      const $table = this.$refs.xTable
      $table.clearActived().then(() => {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.$XModal.message({ content: '保存成功！', status: 'success' })
        }, 300)
      })
    },
    cancelRowEvent (row) {
      const $table = this.$refs.xTable
      $table.clearActived().then(() => {
        // 还原行数据
        $table.revertData(row)
      })
    }
  }
}
</script>
