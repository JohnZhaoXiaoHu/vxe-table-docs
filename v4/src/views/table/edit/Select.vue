<template>
  <div>
    <p class="tip">单元格点击编辑，还可以通过 <table-column-api-link prop="autoselect"/> 开启默认选中</p>

    <vxe-table
      border
      resizable
      show-overflow
      :data="demo1.tableData"
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
          <vxe-select v-model="row.sex" type="text" :optionGroups="demo1.sexGriupList" transfer></vxe-select>
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
      <pre-code class="typescript">{{ demoCodes[1] }}</pre-code>
    </pre>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue'

export default defineComponent({
  setup () {
    const demo1 = reactive({
      tableData: [
        { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: '0', sex2: ['0'], num1: 40, age: 28, address: 'Shenzhen', date12: '', date13: '' },
        { id: 10002, name: 'Test2', nickname: 'T2', role: 'Designer', sex: '1', sex2: ['0', '1'], num1: 20, age: 22, address: 'Guangzhou', date12: '', date13: '2020-08-20' },
        { id: 10003, name: 'Test3', nickname: 'T3', role: 'Test', sex: '0', sex2: ['1'], num1: 200, age: 32, address: 'Shanghai', date12: '2020-09-10', date13: '' },
        { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: '1', sex2: ['1'], num1: 30, age: 23, address: 'Shenzhen', date12: '', date13: '2020-12-04' }
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
    })

    const formatGroupSex = (value: any) => {
      let label = ''
      demo1.sexGriupList.find(group => {
        group.options.find(item => {
          if (item.value === value) {
            label = item.label
            return true
          }
        })
      })
      return label
    }

    return {
      demo1,
      formatGroupSex,
      demoCodes: [
        `
        <vxe-table
          border
          resizable
          show-overflow
          :data="demo1.tableData"
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
              <vxe-select v-model="row.sex" type="text" :optionGroups="demo1.sexGriupList" transfer></vxe-select>
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
        `,
        `
        import { defineComponent, reactive } from 'vue'

        export default defineComponent({
          setup () {
            const demo1 = reactive({
              tableData: [],
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
            })

            const formatGroupSex = (value: any) => {
              let label = ''
              demo1.sexGriupList.find(group => {
                group.options.find(item => {
                  if (item.value === value) {
                    label = item.label
                    return true
                  }
                })
              })
              return label
            }

            return {
              demo1,
              formatGroupSex
            }
          }
        })
        `
      ]
    }
  }
})
</script>
