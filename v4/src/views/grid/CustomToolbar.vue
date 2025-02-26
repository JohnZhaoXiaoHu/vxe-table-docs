<template>
  <div>
    <p class="tip">工具栏：通过 <grid-api-link prop="toolbar"/> 属性开启，还可以使用 <grid-api-link prop="slot"/> 插槽自定义模板</p>

    <vxe-grid v-bind="gridOptions">
      <template #toolbar_buttons>
        <vxe-input v-model="searchName" placeholder="搜索"></vxe-input>
        <vxe-button status="primary">搜索</vxe-button>
        <vxe-button>刷新</vxe-button>
        <vxe-button>新增</vxe-button>
        <vxe-button>保存</vxe-button>
        <vxe-button>导出</vxe-button>
      </template>

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
import { defineComponent, reactive } from 'vue'
import { VxeGridProps } from 'vxe-table'

export default defineComponent({
  setup () {
    const gridOptions = reactive<VxeGridProps>({
      border: true,
      resizable: true,
      keepSource: true,
      showOverflow: true,
      height: 530,
      id: 'toolbar_demo_2',
      loading: false,
      customConfig: {
        storage: true
      },
      editConfig: {
        trigger: 'click',
        mode: 'row',
        showStatus: true
      },
      toolbarConfig: {
        custom: true,
        slots: {
          buttons: 'toolbar_buttons'
        }
      },
      columns: [
        { type: 'checkbox', width: 50 },
        { type: 'seq', width: 60 },
        { field: 'name', title: 'Name', editRender: { autofocus: '.vxe-input--inner' }, slots: { edit: 'name_edit' } },
        {
          title: '分类',
          children: [
            { field: 'nickname', title: 'Nickname', editRender: {}, slots: { edit: 'nickname_edit' } },
            {
              title: '子类',
              children: [
                { field: 'role', title: 'Role', editRender: {}, slots: { edit: 'role_edit' } }
              ]
            }
          ]
        },
        { field: 'address', title: 'Address', showOverflow: true, editRender: {}, slots: { edit: 'address_edit' } }
      ],
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

    return {
      searchName: '',
      gridOptions,
      demoCodes: [
        `
        <vxe-grid v-bind="gridOptions">
          <template #toolbar_buttons>
            <vxe-input v-model="searchName" placeholder="搜索"></vxe-input>
            <vxe-button status="primary">搜索</vxe-button>
            <vxe-button>刷新</vxe-button>
            <vxe-button>新增</vxe-button>
            <vxe-button>保存</vxe-button>
            <vxe-button>导出</vxe-button>
          </template>

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
        import { VxeGridProps } from 'vxe-table'

        export default defineComponent({
          setup () {
            const gridOptions = reactive<VxeGridProps>({
              border: true,
              resizable: true,
              keepSource: true,
              showOverflow: true,
              height: 530,
              id: 'toolbar_demo_2',
              loading: false,
              customConfig: {
                storage: true
              },
              editConfig: {
                trigger: 'click',
                mode: 'row',
                showStatus: true
              },
              toolbarConfig: {
                custom: true,
                slots: {
                  buttons: 'toolbar_buttons'
                }
              },
              columns: [
                { type: 'checkbox', width: 50 },
                { type: 'seq', width: 60 },
                { field: 'name', title: 'Name', editRender: { autofocus: '.vxe-input--inner' }, slots: { edit: 'name_edit' } },
                {
                  title: '分类',
                  children: [
                    { field: 'nickname', title: 'Nickname', editRender: {}, slots: { edit: 'nickname_edit' } },
                    {
                      title: '子类',
                      children: [
                        { field: 'role', title: 'Role', editRender: {}, slots: { edit: 'role_edit' } }
                      ]
                    }
                  ]
                },
                { field: 'address', title: 'Address', showOverflow: true, editRender: {}, slots: { edit: 'address_edit' } }
              ],
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

            return {
              searchName: '',
              gridOptions
            }
          }
        })
        `
      ]
    }
  }
})
</script>
