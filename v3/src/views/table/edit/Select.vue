<template>
  <div>
    <p class="tip">单元格点击编辑，还可以通过 <table-column-api-link prop="autoselect"/> 开启默认选中</p>

    <vxe-table
      border
      resizable
      show-overflow
      :data="tableData"
      :edit-config="{trigger: 'click', mode: 'cell'}">
      <vxe-column type="seq" width="60"></vxe-column>
      <vxe-colgroup title="分组1">
        <vxe-column field="name" title="Name" :edit-render="{autofocus: '.my-input', autoselect: true}">
          <template #edit="{ row }">
            <input v-model="row.name" type="text" class="my-input">
          </template>
        </vxe-column>
        <vxe-column field="sex" title="Sex" :edit-render="{}">
          <template #default="{ row }">
            <span>{{ formatGroupSex(row.sex) }}</span>
          </template>
          <template #edit="{ row }">
          <vxe-select v-model="row.sex" type="text" :optionGroups="sexGriupList" transfer></vxe-select>
        </template>
        </vxe-column>
      </vxe-colgroup>
      <vxe-colgroup title="分组1">
        <vxe-column field="date13" title="Date" :edit-render="{autofocus: '.my-input', autoselect: true}">
          <template #edit="{ row }">
            <vxe-input v-model="row.date13" type="date" class="my-input"></vxe-input>
          </template>
        </vxe-column>
      </vxe-colgroup>
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
      sexGriupList: [
        {
          label: '第一分组',
          options: [
            { value: '0', label: '女' }
          ]
        },
        {
          label: '第二分组',
          options: [
            { value: '1', label: '男' }
          ]
        },
        {
          label: '其他',
          options: [
            { value: '2', label: '无' }
          ]
        }
      ],
      demoCodes: [
        `
        <vxe-table
          border
          resizable
          show-overflow
          :data="tableData"
          :edit-config="{trigger: 'click', mode: 'cell'}">
          <vxe-column type="seq" width="60"></vxe-column>
          <vxe-colgroup title="分组1">
            <vxe-column field="name" title="Name" :edit-render="{autofocus: '.my-input', autoselect: true}">
              <template #edit="{ row }">
                <input v-model="row.name" type="text" class="my-input">
              </template>
            </vxe-column>
            <vxe-column field="sex" title="Sex" :edit-render="{}">
              <template #edit="{ row }">
              <vxe-select v-model="row.sex" type="text" :optionGroups="sexGriupList" transfer></vxe-select>
            </template>
            </vxe-column>
          </vxe-colgroup>
          <vxe-colgroup title="分组1">
            <vxe-column field="date13" title="Date" :edit-render="{autofocus: '.my-input', autoselect: true}">
              <template #default="{ row }">
                <span>{{ formatGroupSex(row.sex2) }}</span>
              </template>
              <template #edit="{ row }">
                <vxe-input v-model="row.date13" type="date" class="my-input"></vxe-input>
              </template>
            </vxe-column>
          </vxe-colgroup>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
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
              sexGriupList: [
                {
                  label: '第一分组',
                  options: [
                    { value: '0', label: '女' }
                  ]
                },
                {
                  label: '第二分组',
                  options: [
                    { value: '1', label: '男' }
                  ]
                },
                {
                  label: '其他',
                  options: [
                    { value: '2', label: '无' }
                  ]
                }
              ]
            }
          },
          methods: {
            formatGroupSex (value) {
              let label = ''
              this.sexGriupList.find(group => {
                group.options.find(item => {
                  if (item.value === value) {
                    label = item.label
                    return true
                  }
                })
              })
              return label
            }
          }
        }
        `
      ]
    }
  },
  methods: {
    formatGroupSex (value) {
      let label = ''
      this.sexGriupList.find(group => {
        group.options.find(item => {
          if (item.value === value) {
            label = item.label
            return true
          }
        })
      })
      return label
    }
  }
}
</script>
