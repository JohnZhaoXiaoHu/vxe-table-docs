<template>
  <div>
    <p class="tip">
      自定义渲染 <a class="link" href="https://www.npmjs.com/package/ant-design-vue" target="_blank">ant-design-vue</a> 组件，自定义渲染需要配合 <table-api-link prop="updateStatus"/> 方法使用，在对应单元格的值发生改变时调用更新状态<br>
      建议通过使用 <a class="link" href="https://github.com/x-extends/vxe-table-plugin-antd/tree/v2" target="_blank">vxe-table-plugin-antd</a> 适配插件来解决跨组件渲染的兼容性问题，例如：无法下拉选中...等<br>
      <span class="red">（注：该示例仅供参考，具体逻辑请自行实现）</span>
    </p>

    <vxe-table
      border
      show-overflow
      show-footer
      keep-source
      ref="xTable"
      height="600"
      class="my-xtable-antd"
      :loading="loading"
      :data="tableData"
      :footer-method="footerMethod"
      :edit-config="{trigger: 'click', mode: 'row', showStatus: true}">
      <vxe-column type="checkbox" width="60"></vxe-column>
      <vxe-column type="seq" width="80">
        <template #header>
          <span>序号</span>
          <a-icon type="question" />
        </template>
      </vxe-column>
      <vxe-column field="name" title="AInput" min-width="140" :edit-render="{}">
        <template #edit="scope">
          <a-input v-model="scope.row.name" @input="$refs.xTable.updateStatus(scope)"></a-input>
        </template>
      </vxe-column>
      <vxe-column field="role" title="AAutoComplete" min-width="160" :edit-render="{}">
        <template #edit="{ row }">
          <a-auto-complete v-model="row.role" :dataSource="dataSource" @select="onSelect" @search="handleSearch"/>
        </template>
      </vxe-column>
      <vxe-column field="age" title="AInputNumber"  width="160" :edit-render="{}">
        <template #header="{ column }">
          <span>{{ column.title }}</span>
          <a-icon type="warning" />
        </template>
        <template #edit="{ row }">
          <a-input-number v-model="row.age" :max="35" :min="18"></a-input-number>
        </template>
      </vxe-column>
      <vxe-column field="sex" title="ASelect" width="140" :edit-render="{}">
        <template #edit="scope">
          <a-select v-model="scope.row.sex" @change="$refs.xTable.updateStatus(scope)">
            <a-select-option v-for="item in sexList" :key="item.value" :value="item.value">{{ item.label }}</a-select-option>
          </a-select>
        </template>
        <template #default="{ row }">{{ getSelectLabel(row.sex, sexList) }}</template>
      </vxe-column>
      <vxe-column field="sex1" title="ASelect" width="180" :edit-render="{}">
        <template #edit="scope">
          <a-select v-model="scope.row.sex1" @change="$refs.xTable.updateStatus(scope)" mode="multiple">
            <a-select-option v-for="item in sexList" :key="item.value" :value="item.value">{{ item.label }}</a-select-option>
          </a-select>
        </template>
        <template #default="{ row }">{{ getSelectMultipleLabel(row.sex1, sexList) }}</template>
      </vxe-column>
      <vxe-column field="region" title="ACascader" width="200" :edit-render="{}">
        <template #edit="{ row }">
          <a-cascader v-model="row.region" :options="regionList"></a-cascader>
        </template>
        <template #default="{ row }">{{ getCascaderLabel(row.region, regionList) }}</template>
      </vxe-column>
      <vxe-column field="date7" title="ADatePicker" width="200" :edit-render="{}">
        <template #edit="{ row }">
          <a-date-picker v-model="row.date7" format="YYYY/MM/DD hh:mm:ss"></a-date-picker>
        </template>
        <template #default="{ row }">{{ formatDate(row.date7, 'YYYY/MM/DD hh:mm:ss') }}</template>
      </vxe-column>
      <vxe-column field="rate" title="ARate" width="200">
        <template #default="{ row }">
          <a-rate v-model="row.rate"></a-rate>
        </template>
      </vxe-column>
      <vxe-column field="flag" title="ElSwitch" width="100">
        <template #default="{ row }">
          <a-switch v-model="row.flag"></a-switch>
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
      dataSource: [],
      sexList: [
        { value: '1', label: '男' },
        { value: '0', label: '女' }
      ],
      regionList: [],
      restaurants: [
        { value: '前端', name: '前端' },
        { value: '后端', name: '后端' }
      ],
      tableData: [],
      demoCodes: [
        `
        <vxe-table
          border
          show-overflow
          show-footer
          keep-source
          ref="xTable"
          height="600"
          class="my-xtable-antd"
          :loading="loading"
          :data="tableData"
          :footer-method="footerMethod"
          :edit-config="{trigger: 'click', mode: 'row', showStatus: true}">
          <vxe-column type="checkbox" width="60"></vxe-column>
          <vxe-column type="seq" width="80">
            <template #header>
              <span>序号</span>
              <a-icon type="question" />
            </template>
          </vxe-column>
          <vxe-column field="name" title="AInput" min-width="140" :edit-render="{}">
            <template #edit="scope">
              <a-input v-model="scope.row.name" @input="$refs.xTable.updateStatus(scope)"></a-input>
            </template>
          </vxe-column>
          <vxe-column field="role" title="AAutoComplete" min-width="160" :edit-render="{}">
            <template #edit="{ row }">
              <a-auto-complete v-model="row.role" :dataSource="dataSource" @select="onSelect" @search="handleSearch"/>
            </template>
          </vxe-column>
          <vxe-column field="age" title="AInputNumber"  width="160" :edit-render="{}">
            <template #header="{ column }">
              <span>{{ column.title }}</span>
              <a-icon type="warning" />
            </template>
            <template #edit="{ row }">
              <a-input-number v-model="row.age" :max="35" :min="18"></a-input-number>
            </template>
          </vxe-column>
          <vxe-column field="sex" title="ASelect" width="140" :edit-render="{}">
            <template #edit="scope">
              <a-select v-model="scope.row.sex" @change="$refs.xTable.updateStatus(scope)">
                <a-select-option v-for="item in sexList" :key="item.value" :value="item.value">{{ item.label }}</a-select-option>
              </a-select>
            </template>
            <template #default="{ row }">{{ getSelectLabel(row.sex, sexList) }}</template>
          </vxe-column>
          <vxe-column field="sex1" title="ASelect" width="180" :edit-render="{}">
            <template #edit="scope">
              <a-select v-model="scope.row.sex1" @change="$refs.xTable.updateStatus(scope)" mode="multiple">
                <a-select-option v-for="item in sexList" :key="item.value" :value="item.value">{{ item.label }}</a-select-option>
              </a-select>
            </template>
            <template #default="{ row }">{{ getSelectMultipleLabel(row.sex1, sexList) }}</template>
          </vxe-column>
          <vxe-column field="region" title="ACascader" width="200" :edit-render="{}">
            <template #edit="{ row }">
              <a-cascader v-model="row.region" :options="regionList"></a-cascader>
            </template>
            <template #default="{ row }">{{ getCascaderLabel(row.region, regionList) }}</template>
          </vxe-column>
          <vxe-column field="date7" title="ADatePicker" width="200" :edit-render="{}">
            <template #edit="{ row }">
              <a-date-picker v-model="row.date7" format="YYYY/MM/DD hh:mm:ss"></a-date-picker>
            </template>
            <template #default="{ row }">{{ formatDate(row.date7, 'YYYY/MM/DD hh:mm:ss') }}</template>
          </vxe-column>
          <vxe-column field="rate" title="ARate" width="200">
            <template #default="{ row }">
              <a-rate v-model="row.rate"></a-rate>
            </template>
          </vxe-column>
          <vxe-column field="flag" title="ElSwitch" width="100">
            <template #default="{ row }">
              <a-switch v-model="row.flag"></a-switch>
            </template>
          </vxe-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              loading: false,
              dataSource: [],
              sexList: [],
              regionList: [],
              restaurants: [
                { value: '前端', name: '前端' },
                { value: '后端', name: '后端' }
              ],
              tableData: []
            }
          },
          created () {
            this.loading = true
            setTimeout(() => {
              this.tableData = [
                { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: '0', sex1: [], region: [], age: 28, date: '', date1: '', date2: '', date7: '', color1: '', rate: 5, flag: false, address: 'Shenzhen' },
                { id: 10002, name: 'Test2', nickname: 'T2', role: 'Test', sex: '1', sex1: [], region: [], age: 22, date: '', date1: '', date2: '', date7: '', color1: '', rate: 2, flag: false, address: 'Guangzhou' },
                { id: 10003, name: 'Test3', nickname: 'T3', role: 'PM', sex: '0', sex1: [], region: [], age: 32, date: '', date1: '', date2: '', date7: '', color1: '', rate: 3, flag: false, address: 'Shanghai' },
                { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: '0', sex1: ['1', '0'], region: [], age: 23, date: '', date1: '', date2: '', color1: '', date7: '', rate: 3, flag: true, address: 'Shenzhen' },
                { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: '0', sex1: ['1', '0'], region: [], age: 30, date: '', date1: '', date2: '', color1: '', date7: '', rate: 0, flag: true, address: 'Shanghai' },
                { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: '0', sex1: [], region: [], age: 21, date: '', date1: '', date2: '', date7: '', color1: '', rate: 3, flag: false, address: 'Shenzhen' },
                { id: 10007, name: 'Test7', nickname: 'T7', role: 'Test', sex: '1', sex1: ['1'], region: [], age: 29, date: '', date1: '', date2: '', date7: '', color1: '', rate: 0, flag: true, address: 'Guangzhou' },
                { id: 10008, name: 'Test8', nickname: 'T8', role: 'Develop', sex: '1', sex1: [], region: [], age: 35, date: '', date1: '', date2: '', date7: '', color1: '', rate: 2, flag: false, address: 'Shenzhen' },
                { id: 10009, name: 'Test9', nickname: 'T9', role: 'Test', sex: '1', sex1: ['0'], region: [], age: 24, date: '', date1: '', date2: '', date7: '', color1: '', rate: 3, flag: false, address: 'Shenzhen' },
                { id: 100010, name: 'Test10', nickname: 'T10', role: 'Develop', sex: '1', sex1: [], region: [], age: 20, date: '', date1: '', date2: '', date7: '', color1: '', rate: 4, flag: false, address: 'Guangzhou' }
              ]
              this.loading = false
            }, 500)
          },
          methods: {
            formatDate (value, format) {
              return value ? value.format(format) : null
            },
            getSelectLabel (value, list, valueProp = 'value', labelField = 'label') {
              const item = list.find(item => item[valueProp] === value)
              return item ? item[labelField] : null
            },
            getSelectMultipleLabel (value, list, valueProp = 'value', labelField = 'label') {
              return value.map(val => {
                const item = list.find(item => item[valueProp] === val)
                return item ? item[labelField] : null
              }).join(', ')
            },
            getCascaderLabel (value, list) {
              const values = value || []
              const labels = []
              const matchCascaderData = function (index, list) {
                const val = values[index]
                if (list && values.length > index) {
                  list.forEach(item => {
                    if (item.value === val) {
                      labels.push(item.label)
                      matchCascaderData(++index, item.children)
                    }
                  })
                }
              }
              matchCascaderData(0, list)
              return labels.join(' / ')
            },
            handleSearch (value) {
              this.dataSource = !value ? [] : [
                value,
                value + value,
                value + value + value
              ]
            },
            onSelect (value) {
              console.log('onSelect', value)
            },
            createStateFilter (queryString) {
              return (state) => {
                return (state.name.toLowerCase().indexOf(queryString.toLowerCase()) === 0)
              }
            },
            meanNum (list, field) {
              let count = 0
              list.forEach(item => {
                count += Number(item[field])
              })
              return count / list.length
            },
            sumNum (list, field) {
              let count = 0
              list.forEach(item => {
                count += Number(item[field])
              })
              return count
            },
            footerMethod ({ columns, data }) {
              return [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '平均'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.meanNum(data, column.property)
                  }
                  return null
                }),
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '和值'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.sumNum(data, column.property)
                  }
                  return null
                })
              ]
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.loading = true
    setTimeout(() => {
      this.tableData = [
        { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: '0', sex1: [], region: [], age: 28, date: '', date1: '', date2: '', date7: '', color1: '', rate: 5, flag: false, address: 'Shenzhen' },
        { id: 10002, name: 'Test2', nickname: 'T2', role: 'Test', sex: '1', sex1: [], region: [], age: 22, date: '', date1: '', date2: '', date7: '', color1: '', rate: 2, flag: false, address: 'Guangzhou' },
        { id: 10003, name: 'Test3', nickname: 'T3', role: 'PM', sex: '0', sex1: [], region: [], age: 32, date: '', date1: '', date2: '', date7: '', color1: '', rate: 3, flag: false, address: 'Shanghai' },
        { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: '0', sex1: ['1', '0'], region: [], age: 23, date: '', date1: '', date2: '', color1: '', date7: '', rate: 3, flag: true, address: 'Shenzhen' },
        { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: '0', sex1: ['1', '0'], region: [], age: 30, date: '', date1: '', date2: '', color1: '', date7: '', rate: 0, flag: true, address: 'Shanghai' },
        { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: '0', sex1: [], region: [], age: 21, date: '', date1: '', date2: '', date7: '', color1: '', rate: 3, flag: false, address: 'Shenzhen' },
        { id: 10007, name: 'Test7', nickname: 'T7', role: 'Test', sex: '1', sex1: ['1'], region: [], age: 29, date: '', date1: '', date2: '', date7: '', color1: '', rate: 0, flag: true, address: 'Guangzhou' },
        { id: 10008, name: 'Test8', nickname: 'T8', role: 'Develop', sex: '1', sex1: [], region: [], age: 35, date: '', date1: '', date2: '', date7: '', color1: '', rate: 2, flag: false, address: 'Shenzhen' },
        { id: 10009, name: 'Test9', nickname: 'T9', role: 'Test', sex: '1', sex1: ['0'], region: [], age: 24, date: '', date1: '', date2: '', date7: '', color1: '', rate: 3, flag: false, address: 'Shenzhen' },
        { id: 100010, name: 'Test10', nickname: 'T10', role: 'Develop', sex: '1', sex1: [], region: [], age: 20, date: '', date1: '', date2: '', date7: '', color1: '', rate: 4, flag: false, address: 'Guangzhou' }
      ]
      this.loading = false
    }, 500)
  },
  methods: {
    formatDate (value, format) {
      return value ? value.format(format) : null
    },
    getSelectLabel (value, list, valueProp = 'value', labelField = 'label') {
      const item = list.find(item => item[valueProp] === value)
      return item ? item[labelField] : null
    },
    getSelectMultipleLabel (value, list, valueProp = 'value', labelField = 'label') {
      return value.map(val => {
        const item = list.find(item => item[valueProp] === val)
        return item ? item[labelField] : null
      }).join(', ')
    },
    getCascaderLabel (value, list) {
      const values = value || []
      const labels = []
      const matchCascaderData = function (index, list) {
        const val = values[index]
        if (list && values.length > index) {
          list.forEach(item => {
            if (item.value === val) {
              labels.push(item.label)
              matchCascaderData(++index, item.children)
            }
          })
        }
      }
      matchCascaderData(0, list)
      return labels.join(' / ')
    },
    handleSearch (value) {
      this.dataSource = !value ? [] : [
        value,
        value + value,
        value + value + value
      ]
    },
    onSelect (value) {
      console.log('onSelect', value)
    },
    createStateFilter (queryString) {
      return (state) => {
        return (state.name.toLowerCase().indexOf(queryString.toLowerCase()) === 0)
      }
    },
    meanNum (list, field) {
      let count = 0
      list.forEach(item => {
        count += Number(item[field])
      })
      return count / list.length
    },
    sumNum (list, field) {
      let count = 0
      list.forEach(item => {
        count += Number(item[field])
      })
      return count
    },
    footerMethod ({ columns, data }) {
      return [
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '平均'
          }
          if (['age', 'rate'].includes(column.property)) {
            return this.meanNum(data, column.property)
          }
          return null
        }),
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '和值'
          }
          if (['age', 'rate'].includes(column.property)) {
            return this.sumNum(data, column.property)
          }
          return null
        })
      ]
    }
  }
}
</script>
