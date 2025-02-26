<template>
  <div>
    <p class="tip">
      自定义渲染 <a class="link" href="https://www.npmjs.com/package/element-ui" target="_blank">element-ui</a> 组件，自定义渲染需要配合 <table-api-link prop="updateStatus"/> 方法使用，在对应单元格的值发生改变时调用更新状态<br>
      建议通过使用 <a class="link" href="https://github.com/x-extends/vxe-table-plugin-element/tree/v2" target="_blank">vxe-table-plugin-element</a> 适配插件来解决跨组件渲染的兼容性问题，例如：无法下拉选中...等<br>
      <span class="red">（注：该示例仅供参考，具体逻辑请自行实现）</span>
    </p>

    <vxe-table
      border
      show-overflow
      show-footer
      keep-source
      ref="xTable"
      height="600"
      class="my-xtable-element"
      :loading="loading"
      :data="tableData"
      :footer-method="footerMethod"
      :edit-config="{trigger: 'click', mode: 'row', showStatus: true}">
      <vxe-column type="checkbox" width="60"></vxe-column>
      <vxe-column type="seq" width="80">
        <template #header>
          <span>序号</span>
          <i class="el-icon-question"></i>
        </template>
      </vxe-column>
      <vxe-column field="name" title="ElInput" min-width="140" :edit-render="{}">
        <template #edit="scope">
          <el-input v-model="scope.row.name" @input="$refs.xTable.updateStatus(scope)"></el-input>
        </template>
      </vxe-column>
      <vxe-column field="role" title="ElAutocomplete" min-width="160" :edit-render="{}">
        <template #edit="{ row }">
          <el-autocomplete v-model="row.role" :fetch-suggestions="roleFetchSuggestions"></el-autocomplete>
        </template>
      </vxe-column>
      <vxe-column field="age" title="ElInputNumber"  width="160" :edit-render="{}">
        <template #header="{ column }">
          <span>{{ column.title }}</span>
          <i class="el-icon-warning"></i>
        </template>
        <template #edit="{ row }">
          <el-input-number v-model="row.age" :max="35" :min="18"></el-input-number>
        </template>
      </vxe-column>
      <vxe-column field="sex" title="ElSelect" width="140" :edit-render="{}">
        <template #edit="scope">
          <el-select v-model="scope.row.sex" @change="$refs.xTable.updateStatus(scope)">
            <el-option v-for="item in sexList" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
        </template>
        <template #default="{ row }">{{ getSelectLabel(row.sex, sexList) }}</template>
      </vxe-column>
      <vxe-column field="sex1" title="ElSelect" width="180" :edit-render="{}">
        <template #edit="scope">
          <el-select v-model="scope.row.sex1" multiple>
            <el-option v-for="item in sexList" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
        </template>
        <template #default="{ row }">{{ getSelectMultipleLabel(row.sex1, sexList) }}</template>
      </vxe-column>
      <vxe-column field="region" title="ElCascader" width="200" :edit-render="{}">
        <template #edit="{ row }">
          <el-cascader v-model="row.region" :options="regionList"></el-cascader>
        </template>
        <template #default="{ row }">{{ getCascaderLabel(row.region, regionList) }}</template>
      </vxe-column>
      <vxe-column field="date" title="ElDatePicker" width="200" :edit-render="{}">
        <template #edit="{ row }">
          <el-date-picker v-model="row.date" type="date" format="yyyy/MM/dd"></el-date-picker>
        </template>
        <template #default="{ row }">{{ formatDate(row.date, 'yyyy/MM/dd') }}</template>
      </vxe-column>
      <vxe-column field="date1" title="ElDatePicker" width="220" :edit-render="{}">
        <template #edit="{ row }">
          <el-date-picker v-model="row.date1" type="datetime" format="yyyy-MM-dd HH:mm:ss"></el-date-picker>
        </template>
        <template #default="{ row }">{{ formatDate(row.date1, 'yyyy-MM-dd HH:mm:ss') }}</template>
      </vxe-column>
      <vxe-column field="date2" title="ElTimePicker" width="200" :edit-render="{}">
        <template #edit="{ row }">
          <el-time-select v-model="row.date2" :picker-options="{start: '08:30', step: '00:15', end: '18:30'}"></el-time-select>
        </template>
      </vxe-column>
      <vxe-column field="color1" title="ElColorPicker" width="140" :edit-render="{}">
        <template #edit="{ row }">
          <el-color-picker v-model="row.color1"></el-color-picker>
        </template>
      </vxe-column>
      <vxe-column field="rate" title="ElRate" width="200">
        <template #default="{ row }">
          <el-rate v-model="row.rate"></el-rate>
        </template>
      </vxe-column>
      <vxe-column field="flag" title="ElSwitch" width="100">
        <template #default="{ row }">
          <el-switch v-model="row.flag"></el-switch>
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
import XEUtils from 'xe-utils'

export default {
  data () {
    return {
      loading: false,
      tableData: [],
      sexList: [
        { value: '1', label: '男' },
        { value: '0', label: '女' }
      ],
      regionList: [
        {
          label: '北京',
          value: 1,
          children: [
            { value: 3, label: '东城区' },
            { value: 4, label: '西城区' }
          ]
        },
        {
          label: '上海',
          value: 21,
          children: [
            { value: 23, label: '黄浦区' },
            { value: 24, label: '卢湾区' }
          ]
        },
        {
          label: '广东',
          value: 42,
          children: [
            { value: 43, label: '广州市' },
            { value: 67, label: '深圳市' }
          ]
        }
      ],
      restaurants: [
        { value: '前端', name: '前端' },
        { value: '后端', name: '后端' }
      ],
      demoCodes: [
        `
        <vxe-table
          border
          show-overflow
          show-footer
          keep-source
          ref="xTable"
          height="600"
          class="my-xtable-element"
          :loading="loading"
          :data="tableData"
          :footer-method="footerMethod"
          :edit-config="{trigger: 'click', mode: 'row', showStatus: true}">
          <vxe-column type="checkbox" width="60"></vxe-column>
          <vxe-column type="seq" width="80">
            <template #header>
              <span>序号</span>
              <i class="el-icon-question"></i>
            </template>
          </vxe-column>
          <vxe-column field="name" title="ElInput" min-width="140" :edit-render="{}">
            <template #edit="scope">
              <el-input v-model="scope.row.name" @input="$refs.xTable.updateStatus(scope)"></el-input>
            </template>
          </vxe-column>
          <vxe-column field="role" title="ElAutocomplete" min-width="160" :edit-render="{}">
            <template #edit="{ row }">
              <el-autocomplete v-model="row.role" :fetch-suggestions="roleFetchSuggestions"></el-autocomplete>
            </template>
          </vxe-column>
          <vxe-column field="age" title="ElInputNumber"  width="160" :edit-render="{}">
            <template #header="{ column }">
              <span>{{ column.title }}</span>
              <i class="el-icon-warning"></i>
            </template>
            <template #edit="{ row }">
              <el-input-number v-model="row.age" :max="35" :min="18"></el-input-number>
            </template>
          </vxe-column>
          <vxe-column field="sex" title="ElSelect" width="140" :edit-render="{}">
            <template #edit="scope">
              <el-select v-model="scope.row.sex" @change="$refs.xTable.updateStatus(scope)">
                <el-option v-for="item in sexList" :key="item.value" :label="item.label" :value="item.value"></el-option>
              </el-select>
            </template>
            <template #default="{ row }">{{ getSelectLabel(row.sex, sexList) }}</template>
          </vxe-column>
          <vxe-column field="sex1" title="ElSelect" width="180" :edit-render="{}">
            <template #edit="scope">
              <el-select v-model="scope.row.sex1" multiple>
                <el-option v-for="item in sexList" :key="item.value" :label="item.label" :value="item.value"></el-option>
              </el-select>
            </template>
            <template #default="{ row }">{{ getSelectMultipleLabel(row.sex1, sexList) }}</template>
          </vxe-column>
          <vxe-column field="region" title="ElCascader" width="200" :edit-render="{}">
            <template #edit="{ row }">
              <el-cascader v-model="row.region" :options="regionList"></el-cascader>
            </template>
            <template #default="{ row }">{{ getCascaderLabel(row.region, regionList) }}</template>
          </vxe-column>
          <vxe-column field="date" title="ElDatePicker" width="200" :edit-render="{}">
            <template #edit="{ row }">
              <el-date-picker v-model="row.date" type="date" format="yyyy/MM/dd"></el-date-picker>
            </template>
            <template #default="{ row }">{{ formatDate(row.date, 'yyyy/MM/dd') }}</template>
          </vxe-column>
          <vxe-column field="date1" title="ElDatePicker" width="220" :edit-render="{}">
            <template #edit="{ row }">
              <el-date-picker v-model="row.date1" type="datetime" format="yyyy-MM-dd HH:mm:ss"></el-date-picker>
            </template>
            <template #default="{ row }">{{ formatDate(row.date1, 'yyyy-MM-dd HH:mm:ss') }}</template>
          </vxe-column>
          <vxe-column field="date2" title="ElTimePicker" width="200" :edit-render="{}">
            <template #edit="{ row }">
              <el-time-select v-model="row.date2" :picker-options="{start: '08:30', step: '00:15', end: '18:30'}"></el-time-select>
            </template>
          </vxe-column>
          <vxe-column field="color1" title="ElColorPicker" width="140" :edit-render="{}">
            <template #edit="{ row }">
              <el-color-picker v-model="row.color1"></el-color-picker>
            </template>
          </vxe-column>
          <vxe-column field="rate" title="ElRate" width="200">
            <template #default="{ row }">
              <el-rate v-model="row.rate"></el-rate>
            </template>
          </vxe-column>
          <vxe-column field="flag" title="ElSwitch" width="100">
            <template #default="{ row }">
              <el-switch v-model="row.flag"></el-switch>
            </template>
          </vxe-column>
        </vxe-table>
        `,
        `
        import XEUtils from 'xe-utils'
        
        export default {
          data () {
            return {
              loading: false,
              tableData: [],
              sexList: [],
              regionList: [],
              restaurants: [
                { value: '前端', name: '前端' },
                { value: '后端', name: '后端' }
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
              return XEUtils.toDateString(value, format)
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
            roleFetchSuggestions (queryString, cb) {
              const restaurants = this.restaurants
              const results = queryString ? restaurants.filter(this.createStateFilter(queryString)) : restaurants
              clearTimeout(this.timeout)
              this.timeout = setTimeout(() => {
                cb(results)
              }, 3000 * Math.random())
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
      return XEUtils.toDateString(value, format)
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
    roleFetchSuggestions (queryString, cb) {
      const restaurants = this.restaurants
      const results = queryString ? restaurants.filter(this.createStateFilter(queryString)) : restaurants
      clearTimeout(this.timeout)
      this.timeout = setTimeout(() => {
        cb(results)
      }, 3000 * Math.random())
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
