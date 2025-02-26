<template>
  <div>
    <p class="tip">
      通过表尾来实现合计功能，数据发生变化时实时统计，对于某些场景下如果需要频繁计算的可以手动调用 <table-api-link prop="updateFooter"/> 函数<br>
      如果是内置的渲染器，可以设置 <table-column-api-link prop="immediate"/> 属性和相关事件去实时更新<br>
      <span class="red">（注：实时更新是非常糟糕的做法，运算量越大卡顿就越久，非特殊场景不建议使用）</span>
    </p>

    <vxe-toolbar ref="xToolbar" export>
      <template #buttons>
        <vxe-button @click="insertEvent">新增</vxe-button>
        <vxe-button @click="removeEvent">删除</vxe-button>
        <vxe-button @click="saveEvent">保存</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      border
      show-footer
      show-overflow
      highlight-hover-row
      ref="xTable"
      height="400"
      class="editable-footer"
      :export-config="{}"
      :footer-method="footerMethod"
      :footer-cell-class-name="footerCellClassName"
      :data="demo1.tableData"
      :edit-config="{trigger: 'click', mode: 'row'}">
      <vxe-column type="seq" width="60"></vxe-column>
      <vxe-colgroup title="统计信息">
        <vxe-column field="name" title="Name" :edit-render="{name: 'input', immediate: true}">
          <template #edit="{ row }">
            <vxe-input v-model="row.name" type="text"></vxe-input>
          </template>
        </vxe-column>
        <vxe-column field="age" title="Age" :edit-render="{autofocus: '.vxe-input--inner'}">
          <template #edit="{ row }">
            <vxe-input v-model="row.age" type="text" :min="1" :max="120" @change="updateFooterEvent"></vxe-input>
          </template>
        </vxe-column>
        <vxe-column field="num1" title="Num" :edit-render="{autofocus: '.vxe-input--inner'}">
          <template #edit="{ row }">
            <vxe-input v-model="row.num1" type="text" @input="updateFooterEvent"></vxe-input>
          </template>
        </vxe-column>
        <vxe-column field="rate" title="Rate" :edit-render="{}">
          <template #edit="{ row }">
            <vxe-input v-model="row.rate" type="text" @input="updateFooterEvent"></vxe-input>
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
import { defineComponent, reactive, ref, nextTick } from 'vue'
import { VXETable, VxeTableInstance, VxeTablePropTypes, VxeToolbarInstance } from 'vxe-table'

export default defineComponent({
  setup () {
    const xTable = ref({} as VxeTableInstance)
    const xToolbar = ref({} as VxeToolbarInstance)

    const demo1 = reactive({
      tableData: [
        { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: '0', sex2: ['0'], num1: 40, age: 28, rate: 22 },
        { id: 10002, name: 'Test2', nickname: 'T2', role: 'Designer', sex: '1', sex2: ['0', '1'], num1: 23, age: 22, rate: 34 },
        { id: 10003, name: 'Test3', nickname: 'T3', role: 'Test', sex: '0', sex2: ['1'], num1: 200, age: 32, rate: 18 },
        { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: '1', sex2: ['1'], num1: 30, age: 23, rate: 13 },
        { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: '0', sex2: ['1', '0'], num1: 20, age: 30, rate: 6 },
        { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: '1', sex2: ['0'], num1: 10, age: 21, rate: 33 },
        { id: 10007, name: 'Test7', nickname: 'T7', role: 'Develop', sex: '0', sex2: ['0'], num1: 5, age: 29, rate: 4 },
        { id: 10008, name: 'Test8', nickname: 'T8', role: 'PM', sex: '1', sex2: ['0'], num1: 2, age: 35, rate: 55 }
      ]
    })

    const footerCellClassName: VxeTablePropTypes.FooterCellClassName = ({ $rowIndex, columnIndex }) => {
      if (columnIndex === 1) {
        if ($rowIndex === 0) {
          return 'col-blue'
        }
      }
      if (columnIndex === 2) {
        if ($rowIndex === 1) {
          return 'col-red'
        }
      }
    }

    // 在值发生改变时更新表尾合计
    const updateFooterEvent = () => {
      const $table = xTable.value
      $table.updateFooter()
    }

    const meanNum = (list: any[], field: string) => {
      let count = 0
      list.forEach(item => {
        count += Number(item[field])
      })
      return count / list.length
    }

    const sumNum = (list: any[], field: string) => {
      let count = 0
      list.forEach(item => {
        count += Number(item[field])
      })
      return count
    }

    const footerMethod: VxeTablePropTypes.FooterMethod = ({ columns, data }) => {
      return [
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '平均'
          }
          if (['age'].includes(column.property)) {
            return meanNum(data, column.property)
          } else if (['rate', 'num1'].includes(column.property)) {
            return meanNum(data, column.property)
          }
          return null
        }),
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '和值'
          }
          if (['rate', 'num1'].includes(column.property)) {
            return sumNum(data, column.property)
          }
          return null
        })
      ]
    }

    const insertEvent = async () => {
      const record = {
        name: 'New name'
      }
      const $table = xTable.value
      const { row: newRow } = await $table.insert(record)
      $table.setActiveCell(newRow, 'age')
    }

    const removeEvent = () => {
      const $table = xTable.value
      $table.removeCheckboxRow()
    }

    const saveEvent = () => {
      const $table = xTable.value
      const { insertRecords, removeRecords, updateRecords } = $table.getRecordset()
      VXETable.modal.alert(`insertRecords=${insertRecords.length} removeRecords=${removeRecords.length} updateRecords=${updateRecords.length}`)
    }

    nextTick(() => {
      // 将表格和工具栏进行关联
      const $table = xTable.value
      const $toolbar = xToolbar.value
      $table.connect($toolbar)
    })

    return {
      xTable,
      xToolbar,
      demo1,
      footerCellClassName,
      updateFooterEvent,
      footerMethod,
      insertEvent,
      removeEvent,
      saveEvent,
      demoCodes: [
        `
        <vxe-toolbar ref="xToolbar" export>
          <template #buttons>
            <vxe-button @click="insertEvent">新增</vxe-button>
            <vxe-button @click="removeEvent">删除</vxe-button>
            <vxe-button @click="saveEvent">保存</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          border
          show-footer
          show-overflow
          highlight-hover-row
          ref="xTable"
          height="400"
          class="editable-footer"
          :export-config="{}"
          :footer-method="footerMethod"
          :footer-cell-class-name="footerCellClassName"
          :data="demo1.tableData"
          :edit-config="{trigger: 'click', mode: 'row'}">
          <vxe-column type="seq" width="60"></vxe-column>
          <vxe-colgroup title="统计信息">
            <vxe-column field="name" title="Name" :edit-render="{name: 'input', immediate: true}">
              <template #edit="{ row }">
                <vxe-input v-model="row.name" type="text"></vxe-input>
              </template>
            </vxe-column>
            <vxe-column field="age" title="Age" :edit-render="{autofocus: '.vxe-input--inner'}">
              <template #edit="{ row }">
                <vxe-input v-model="row.age" type="text" :min="1" :max="120" @change="updateFooterEvent"></vxe-input>
              </template>
            </vxe-column>
            <vxe-column field="num1" title="Num" :edit-render="{autofocus: '.vxe-input--inner'}">
              <template #edit="{ row }">
                <vxe-input v-model="row.num1" type="text" @input="updateFooterEvent"></vxe-input>
              </template>
            </vxe-column>
            <vxe-column field="rate" title="Rate" :edit-render="{}">
              <template #edit="{ row }">
                <vxe-input v-model="row.rate" type="text" @input="updateFooterEvent"></vxe-input>
              </template>
            </vxe-column>
          </vxe-colgroup>
        </vxe-table>
        `,
        `
        import { defineComponent, reactive, ref, nextTick } from 'vue'
        import { VXETable, VxeTableInstance, VxeTablePropTypes, VxeToolbarInstance } from 'vxe-table'

        export default defineComponent({
          setup () {
            const xTable = ref({} as VxeTableInstance)
            const xToolbar = ref({} as VxeToolbarInstance)

            const demo1 = reactive({
              tableData: [
                { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: '0', sex2: ['0'], num1: 40, age: 28, rate: 22 },
                { id: 10002, name: 'Test2', nickname: 'T2', role: 'Designer', sex: '1', sex2: ['0', '1'], num1: 23, age: 22, rate: 34 },
                { id: 10003, name: 'Test3', nickname: 'T3', role: 'Test', sex: '0', sex2: ['1'], num1: 200, age: 32, rate: 18 },
                { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: '1', sex2: ['1'], num1: 30, age: 23, rate: 13 },
                { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: '0', sex2: ['1', '0'], num1: 20, age: 30, rate: 6 },
                { id: 10006, name: 'Test6', nickname: 'T6', role: 'Designer', sex: '1', sex2: ['0'], num1: 10, age: 21, rate: 33 },
                { id: 10007, name: 'Test7', nickname: 'T7', role: 'Develop', sex: '0', sex2: ['0'], num1: 5, age: 29, rate: 4 },
                { id: 10008, name: 'Test8', nickname: 'T8', role: 'PM', sex: '1', sex2: ['0'], num1: 2, age: 35, rate: 55 }
              ]
            })

            const footerCellClassName: VxeTablePropTypes.FooterCellClassName = ({ $rowIndex, columnIndex }) => {
              if (columnIndex === 1) {
                if ($rowIndex === 0) {
                  return 'col-blue'
                }
              }
              if (columnIndex === 2) {
                if ($rowIndex === 1) {
                  return 'col-red'
                }
              }
            }

            // 在值发生改变时更新表尾合计
            const updateFooterEvent = () => {
              const $table = xTable.value
              $table.updateFooter()
            }

            const meanNum = (list: any[], field: string) => {
              let count = 0
              list.forEach(item => {
                count += Number(item[field])
              })
              return count / list.length
            }

            const sumNum = (list: any[], field: string) => {
              let count = 0
              list.forEach(item => {
                count += Number(item[field])
              })
              return count
            }

            const footerMethod: VxeTablePropTypes.FooterMethod = ({ columns, data }) => {
              return [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '平均'
                  }
                  if (['age'].includes(column.property)) {
                    return meanNum(data, column.property)
                  } else if (['rate', 'num1'].includes(column.property)) {
                    return meanNum(data, column.property)
                  }
                  return null
                }),
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '和值'
                  }
                  if (['rate', 'num1'].includes(column.property)) {
                    return sumNum(data, column.property)
                  }
                  return null
                })
              ]
            }

            const insertEvent = async () => {
              const record = {
                name: 'New name'
              }
              const $table = xTable.value
              const { row: newRow } = await $table.insert(record)
              $table.setActiveCell(newRow, 'age')
            }

            const removeEvent = () => {
              const $table = xTable.value
              $table.removeCheckboxRow()
            }

            const saveEvent = () => {
              const $table = xTable.value
              const { insertRecords, removeRecords, updateRecords } = $table.getRecordset()
              VXETable.modal.alert(\`insertRecords=\${insertRecords.length} removeRecords=\${removeRecords.length} updateRecords=\${updateRecords.length}\`)
            }

            nextTick(() => {
              // 将表格和工具栏进行关联
              const $table = xTable.value
              const $toolbar = xToolbar.value
              $table.connect($toolbar)
            })

            return {
              xTable,
              xToolbar,
              demo1,
              footerCellClassName,
              updateFooterEvent,
              footerMethod,
              insertEvent,
              removeEvent,
              saveEvent
            }
          }
        })
        `
      ]
    }
  }
})
</script>

<style>
.editable-footer .vxe-footer--column.col-blue {
  background-color: #2db7f5;
  color: #fff;
}
.editable-footer .vxe-footer--column.col-red {
  background-color: red;
  color: #fff;
}
</style>
