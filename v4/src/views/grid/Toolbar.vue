<template>
  <div>
    <p class="tip">工具栏：通过 <grid-api-link prop="toolbar-config"/> 属性配置，支持显示/隐藏列、列宽拖动状态的保存功能，可以通过表格的 <table-api-link prop="custom-config"/> 参数开启将列个性化的设置状态保存到本地</p>

    <vxe-grid ref="xGrid" v-bind="gridOptions" v-on="gridEvents">
      <template #name_edit="{ row }">
        <vxe-input v-model="row.name"></vxe-input>
      </template>
      <template #nickname_edit="{ row }">
        <vxe-input v-model="row.nickname"></vxe-input>
      </template>
      <template #role_edit="{ row }">
        <vxe-input v-model="row.role"></vxe-input>
      </template>
      <template #address_edit="{ row }">
        <vxe-input v-model="row.address"></vxe-input>
      </template>
    </vxe-grid>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="xml">{{ demoCodes[0] }}</pre-code>
      <pre-code class="typescript">{{ demoCodes[1] }}</pre-code>
    </pre>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'
import { VXETable, VxeGridInstance, VxeGridListeners, VxeGridProps } from 'vxe-table'

export default defineComponent({
  setup () {
    const xGrid = ref({} as VxeGridInstance)

    const gridOptions = reactive<VxeGridProps>({
      border: true,
      resizable: true,
      keepSource: true,
      id: 'toolbar_demo_1',
      height: 530,
      printConfig: {},
      importConfig: {},
      exportConfig: {},
      customConfig: {
        storage: true
      },
      editConfig: {
        trigger: 'click',
        mode: 'row',
        showStatus: true
      },
      columns: [
        { type: 'checkbox', width: 50 },
        { type: 'seq', width: 60 },
        { field: 'name', title: 'Name', editRender: {}, slots: { edit: 'name_edit' } },
        {
          title: '分类',
          children: [
            { field: 'nickname', title: 'Nickname', editRender: { autofocus: '.vxe-input--inner' }, slots: { edit: 'nickname_edit' } },
            { field: 'role', title: 'Role', editRender: {}, slots: { edit: 'role_edit' } }
          ]
        },
        { field: 'address', title: 'Address', showOverflow: true, editRender: {}, slots: { edit: 'address_edit' } }
      ],
      toolbarConfig: {
        buttons: [
          { code: 'myInsert', name: '新增' },
          { code: 'mySave', name: 'app.body.button.save', status: 'success' },
          { code: 'myExport', name: '导出数据', type: 'text', status: 'warning' },
          {
            name: '禁用按钮',
            disabled: false,
            dropdowns: [
              { code: 'other1', name: '下拉的按钮1', type: 'text', disabled: false },
              { code: 'other2', name: '下拉的按钮2', type: 'text', disabled: true },
              { code: 'other3', name: '下拉的按钮3', type: 'text', disabled: false }
            ]
          }
        ],
        tools: [
          { code: 'myPrint', name: '自定义打印' }
        ],
        import: true,
        export: true,
        print: true,
        zoom: true,
        custom: true
      },
      data: [
        { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: 'Man', age: 28, address: 'Shenzhen' },
        { id: 10002, name: 'Test2', nickname: 'T2', role: 'Test', sex: 'Women', age: 22, address: 'Guangzhou' },
        { id: 10003, name: 'Test3', nickname: 'T3', role: 'PM', sex: 'Man', age: 32, address: 'Shanghai' },
        { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: 'Women', age: 23, address: 'Shenzhen' },
        { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: 'Women', age: 30, address: 'Shanghai' },
        { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: 'Women', age: 21, address: 'Shenzhen' },
        { id: 10007, name: 'Test7', nickname: 'T7', role: 'Test', sex: 'Man', age: 29, address: 'Shenzhen' },
        { id: 10008, name: 'Test8', nickname: 'T8', role: 'Develop', sex: 'Man', age: 35, address: 'Shenzhen' }
      ]
    })

    const gridEvents: VxeGridListeners = {
      toolbarButtonClick ({ code }) {
        const $grid = xGrid.value
        switch (code) {
          case 'myInsert': {
            $grid.insert({
              name: 'xxx'
            })
            break
          }
          case 'mySave': {
            const { insertRecords, removeRecords, updateRecords } = $grid.getRecordset()
            VXETable.modal.message({ content: `新增 ${insertRecords.length} 条，删除 ${removeRecords.length} 条，更新 ${updateRecords.length} 条`, status: 'success' })
            break
          }
          case 'myExport': {
            $grid.exportData({
              type: 'csv'
            })
            break
          }
        }
      },
      toolbarToolClick ({ code }) {
        const $grid = xGrid.value
        switch (code) {
          case 'myPrint': {
            $grid.print()
            break
          }
        }
      }
    }

    return {
      xGrid,
      gridOptions,
      gridEvents,
      demoCodes: [
        `
        <vxe-grid ref="xGrid" v-bind="gridOptions" v-on="gridEvents">
          <template #name_edit="{ row }">
            <vxe-input v-model="row.name"></vxe-input>
          </template>
          <template #nickname_edit="{ row }">
            <vxe-input v-model="row.nickname"></vxe-input>
          </template>
          <template #role_edit="{ row }">
            <vxe-input v-model="row.role"></vxe-input>
          </template>
          <template #address_edit="{ row }">
            <vxe-input v-model="row.address"></vxe-input>
          </template>
        </vxe-grid>
        `,
        `
        import { defineComponent, reactive, ref } from 'vue'
        import { VXETable, VxeGridInstance, VxeGridListeners, VxeGridProps } from 'vxe-table'

        export default defineComponent({
          setup () {
            const xGrid = ref({} as VxeGridInstance)

            const gridOptions = reactive<VxeGridProps>({
              border: true,
              resizable: true,
              keepSource: true,
              id: 'toolbar_demo_1',
              height: 530,
              printConfig: {},
              importConfig: {},
              exportConfig: {},
              customConfig: {
                storage: true
              },
              editConfig: {
                trigger: 'click',
                mode: 'row',
                showStatus: true
              },
              columns: [
                { type: 'checkbox', width: 50 },
                { type: 'seq', width: 60 },
                { field: 'name', title: 'Name', editRender: {}, slots: { edit: 'name_edit' } },
                {
                  title: '分类',
                  children: [
                    { field: 'nickname', title: 'Nickname', editRender: { autofocus: '.vxe-input--inner' }, slots: { edit: 'nickname_edit' } },
                    { field: 'role', title: 'Role', editRender: {}, slots: { edit: 'role_edit' } }
                  ]
                },
                { field: 'address', title: 'Address', showOverflow: true, editRender: {}, slots: { edit: 'address_edit' } }
              ],
              toolbarConfig: {
                buttons: [
                  { code: 'myInsert', name: '新增' },
                  { code: 'mySave', name: 'app.body.button.save', status: 'success' },
                  { code: 'myExport', name: '导出数据', type: 'text', status: 'warning' },
                  {
                    name: '禁用按钮',
                    disabled: false,
                    dropdowns: [
                      { code: 'other1', name: '下拉的按钮1', type: 'text', disabled: false },
                      { code: 'other2', name: '下拉的按钮2', type: 'text', disabled: true },
                      { code: 'other3', name: '下拉的按钮3', type: 'text', disabled: false }
                    ]
                  }
                ],
                tools: [
                  { code: 'myPrint', name: '自定义打印' }
                ],
                import: true,
                export: true,
                print: true,
                zoom: true,
                custom: true
              },
              data: [
                { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: 'Man', age: 28, address: 'Shenzhen' },
                { id: 10002, name: 'Test2', nickname: 'T2', role: 'Test', sex: 'Women', age: 22, address: 'Guangzhou' },
                { id: 10003, name: 'Test3', nickname: 'T3', role: 'PM', sex: 'Man', age: 32, address: 'Shanghai' },
                { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: 'Women', age: 23, address: 'Shenzhen' },
                { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: 'Women', age: 30, address: 'Shanghai' },
                { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: 'Women', age: 21, address: 'Shenzhen' },
                { id: 10007, name: 'Test7', nickname: 'T7', role: 'Test', sex: 'Man', age: 29, address: 'Shenzhen' },
                { id: 10008, name: 'Test8', nickname: 'T8', role: 'Develop', sex: 'Man', age: 35, address: 'Shenzhen' }
              ]
            })

            const gridEvents: VxeGridListeners = {
              toolbarButtonClick ({ code }) {
                const $grid = xGrid.value
                switch (code) {
                  case 'myInsert': {
                    $grid.insert({
                      name: 'xxx'
                    })
                    break
                  }
                  case 'mySave': {
                    const { insertRecords, removeRecords, updateRecords } = $grid.getRecordset()
                    VXETable.modal.message({ content: \`新增 \${insertRecords.length} 条，删除 \${removeRecords.length} 条，更新 \${updateRecords.length} 条\`, status: 'success' })
                    break
                  }
                  case 'myExport': {
                    $grid.exportData({
                      type: 'csv'
                    })
                    break
                  }
                }
              },
              toolbarToolClick ({ code }) {
                const $grid = xGrid.value
                switch (code) {
                  case 'myPrint': {
                    $grid.print()
                    break
                  }
                }
              }
            }

            return {
              xGrid,
              gridOptions,
              gridEvents
            }
          }
        })
        `
      ]
    }
  }
})
</script>
