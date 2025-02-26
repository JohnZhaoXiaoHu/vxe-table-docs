<template>
  <div>
    <p class="tip">实现横向树列表<br><span class="red">（注：<table-api-link prop="span-method"/> ，不能用于树形结构、展开行、固定列，合并的逻辑都是自行实现的，该示例仅供参考）</span></p>

    <vxe-table
      border
      height="600"
      :scroll-y="{enabled: false}"
      :span-method="rowspanMethod"
      :data="demo1.tableData">
      <vxe-column field="name1" title="功能模块">
        <template #default="{ row }">
          <vxe-checkbox v-model="row.check1" @change="check1ChangeEvent(row, row.check1)">{{ row.name1 }}</vxe-checkbox>
        </template>
      </vxe-column>
      <vxe-column field="name2" title="详细功能">
        <template #default="{ row }">
          <vxe-checkbox v-model="row.check2" @change="check2ChangeEvent(row, row.check2)">{{ row.name2 }}</vxe-checkbox>
        </template>
      </vxe-column>
      <vxe-column field="name3" title="权限类型">
        <template #default="{ row }">
          <vxe-checkbox v-model="row.check3" @change="check3ChangeEvent(row, row.check3)">{{ row.name3 }}</vxe-checkbox>
        </template>
      </vxe-column>
      <vxe-column field="name4" title="权限列表">
        <template #default="{ row }">
          <vxe-checkbox v-model="row.check4" @change="check4ChangeEvent(row, row.check4)">{{ row.name4 }}</vxe-checkbox>
        </template>
      </vxe-column>
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
import { VxeTablePropTypes } from 'vxe-table'
import XEUtils from 'xe-utils'

export default defineComponent({
  setup () {
    const demo1 = reactive({
      tableData: [] as any[]
    })

    const check4ChangeEvent = (row: any, checked: boolean) => {
      let childList = demo1.tableData.filter(item => item.id3 === row.id3 && item.name4 === row.name4)
      childList.forEach(item => {
        item.check4 = checked
      })
      childList = demo1.tableData.filter(item => item.id3 === row.id3)
      const isChecked3 = childList.every(item => item.check4)
      childList.forEach(item => {
        item.check3 = isChecked3
      })
      childList = demo1.tableData.filter(item => item.id2 === row.id2)
      const isChecked2 = childList.every(item => item.check3)
      childList.forEach(item => {
        item.check2 = isChecked2
      })
      childList = demo1.tableData.filter(item => item.id1 === row.id1)
      const isChecked1 = childList.every(item => item.check2)
      childList.forEach(item => {
        item.check1 = isChecked1
      })
    }

    const check3ChangeEvent = (row: any, checked: boolean) => {
      let childList = demo1.tableData.filter(item => item.id2 === row.id2 && item.name3 === row.name3)
      childList.forEach(item => {
        item.check3 = checked
      })
      childList = demo1.tableData.filter(item => item.id3 === row.id3)
      childList.forEach(item => {
        check4ChangeEvent(item, checked)
      })
    }

    const check2ChangeEvent = (row: any, checked: boolean) => {
      let childList = demo1.tableData.filter(item => item.id1 === row.id1 && item.name2 === row.name2)
      childList.forEach(item => {
        item.check2 = checked
      })
      childList = demo1.tableData.filter(item => item.id2 === row.id2)
      childList.forEach(item => {
        check3ChangeEvent(item, checked)
      })
    }

    const check1ChangeEvent = (row: any, checked: boolean) => {
      let childList = demo1.tableData.filter(item => item.name1 === row.name1)
      childList.forEach(item => {
        item.check1 = checked
      })
      childList = demo1.tableData.filter(item => item.id1 === row.id1)
      childList.forEach(item => {
        check2ChangeEvent(item, checked)
      })
    }

    const getList = () => {
      const list = [
        { id: '10000', parentId: null, name: '账号管理' },
        { id: '11000', parentId: '10000', name: '用户管理' },
        { id: '11100', parentId: '11000', name: '查看' },
        { id: '11110', parentId: '11100', name: '用户列表' },
        { id: '11200', parentId: '11000', name: '编辑' },
        { id: '11210', parentId: '11200', name: '用户列表' },
        { id: '11220', parentId: '11200', name: '新增用户' },
        { id: '11300', parentId: '11000', name: '操作' },
        { id: '11310', parentId: '11300', name: '新增' },
        { id: '11320', parentId: '11300', name: '删除' },
        { id: '11330', parentId: '11300', name: '修改' },
        { id: '12000', parentId: '10000', name: '角色管理' },
        { id: '12100', parentId: '12000', name: '查看' },
        { id: '12110', parentId: '12100', name: '角色列表' },
        { id: '12200', parentId: '12000', name: '编辑' },
        { id: '122100', parentId: '12200', name: '角色列表' },
        { id: '12220', parentId: '12200', name: '新增角色' },
        { id: '12300', parentId: '12000', name: '操作' },
        { id: '12310', parentId: '12300', name: '新增' },
        { id: '12320', parentId: '12300', name: '删除' },
        { id: '12330', parentId: '12300', name: '修改' },
        { id: '20000', parentId: null, name: '个人中心' },
        { id: '21000', parentId: '20000', name: '个性化设置' },
        { id: '21100', parentId: '21000', name: '查看' },
        { id: '21110', parentId: '21100', name: '信息列表' },
        { id: '21200', parentId: '21000', name: '操作' },
        { id: '21210', parentId: '21200', name: '重置信息' },
        { id: '22000', parentId: '20000', name: '账户管理' },
        { id: '22100', parentId: '22000', name: '查看' },
        { id: '22110', parentId: '22100', name: '账户余额' },
        { id: '22120', parentId: '22100', name: '帐变记录' }
      ]
      return list
    }

    // 将普通树结构转换为横向树列表
    const toColTreeData = (treeData: any[]) => {
      const options = { children: 'children' }
      const list: any[] = []
      const keyMap: any = {}
      XEUtils.eachTree(treeData, (item, index, result, paths, parent) => {
        keyMap[item.id] = item
        item.keys = parent ? parent.keys.concat([item.id]) : [item.id]
        if (!item.children || !item.children.length) {
          const row: any = {}
          item.keys.forEach((key: any, index: number) => {
            const level = index + 1
            const obj = keyMap[key]
            row[`check${level}`] = false
            row[`id${level}`] = obj.id
            row[`name${level}`] = obj.name
          })
          list.push(row)
        }
      }, options)
      demo1.tableData = list
    }

    // 通用行合并函数（将相同多列数据合并为一行）
    const rowspanMethod: VxeTablePropTypes.SpanMethod = ({ row, _rowIndex, column, visibleData }) => {
      const fields = ['name1', 'name2', 'name3']
      const cellValue = row[column.property]
      if (cellValue && fields.includes(column.property)) {
        const prevRow = visibleData[_rowIndex - 1]
        let nextRow = visibleData[_rowIndex + 1]
        if (prevRow && prevRow[column.property] === cellValue) {
          return { rowspan: 0, colspan: 0 }
        } else {
          let countRowspan = 1
          while (nextRow && nextRow[column.property] === cellValue) {
            nextRow = visibleData[++countRowspan + _rowIndex]
          }
          if (countRowspan > 1) {
            return { rowspan: countRowspan, colspan: 1 }
          }
        }
      }
    }

    const treeData = XEUtils.toArrayTree(getList())
    toColTreeData(treeData)

    return {
      demo1,
      check1ChangeEvent,
      check2ChangeEvent,
      check3ChangeEvent,
      check4ChangeEvent,
      rowspanMethod,
      demoCodes: [
        `
        <vxe-table
          border
          height="600"
          :scroll-y="{enabled: false}"
          :span-method="rowspanMethod"
          :data="demo1.tableData">
          <vxe-column field="name1" title="功能模块">
            <template #default="{ row }">
              <vxe-checkbox v-model="row.check1" @change="check1ChangeEvent(row, row.check1)">{{ row.name1 }}</vxe-checkbox>
            </template>
          </vxe-column>
          <vxe-column field="name2" title="详细功能">
            <template #default="{ row }">
              <vxe-checkbox v-model="row.check2" @change="check2ChangeEvent(row, row.check2)">{{ row.name2 }}</vxe-checkbox>
            </template>
          </vxe-column>
          <vxe-column field="name3" title="权限类型">
            <template #default="{ row }">
              <vxe-checkbox v-model="row.check3" @change="check3ChangeEvent(row, row.check3)">{{ row.name3 }}</vxe-checkbox>
            </template>
          </vxe-column>
          <vxe-column field="name4" title="权限列表">
            <template #default="{ row }">
              <vxe-checkbox v-model="row.check4" @change="check4ChangeEvent(row, row.check4)">{{ row.name4 }}</vxe-checkbox>
            </template>
          </vxe-column>
        </vxe-table>
        `,
        `
        import { defineComponent, reactive } from 'vue'
        import { VxeTablePropTypes } from 'vxe-table'
        import XEUtils from 'xe-utils'

        export default defineComponent({
          setup () {
            const demo1 = reactive({
              tableData: [] as any[]
            })

            const check4ChangeEvent = (row: any, checked: boolean) => {
              let childList = demo1.tableData.filter(item => item.id3 === row.id3 && item.name4 === row.name4)
              childList.forEach(item => {
                item.check4 = checked
              })
              childList = demo1.tableData.filter(item => item.id3 === row.id3)
              const isChecked3 = childList.every(item => item.check4)
              childList.forEach(item => {
                item.check3 = isChecked3
              })
              childList = demo1.tableData.filter(item => item.id2 === row.id2)
              const isChecked2 = childList.every(item => item.check3)
              childList.forEach(item => {
                item.check2 = isChecked2
              })
              childList = demo1.tableData.filter(item => item.id1 === row.id1)
              const isChecked1 = childList.every(item => item.check2)
              childList.forEach(item => {
                item.check1 = isChecked1
              })
            }

            const check3ChangeEvent = (row: any, checked: boolean) => {
              let childList = demo1.tableData.filter(item => item.id2 === row.id2 && item.name3 === row.name3)
              childList.forEach(item => {
                item.check3 = checked
              })
              childList = demo1.tableData.filter(item => item.id3 === row.id3)
              childList.forEach(item => {
                check4ChangeEvent(item, checked)
              })
            }

            const check2ChangeEvent = (row: any, checked: boolean) => {
              let childList = demo1.tableData.filter(item => item.id1 === row.id1 && item.name2 === row.name2)
              childList.forEach(item => {
                item.check2 = checked
              })
              childList = demo1.tableData.filter(item => item.id2 === row.id2)
              childList.forEach(item => {
                check3ChangeEvent(item, checked)
              })
            }

            const check1ChangeEvent = (row: any, checked: boolean) => {
              let childList = demo1.tableData.filter(item => item.name1 === row.name1)
              childList.forEach(item => {
                item.check1 = checked
              })
              childList = demo1.tableData.filter(item => item.id1 === row.id1)
              childList.forEach(item => {
                check2ChangeEvent(item, checked)
              })
            }

            const getList = () => {
              const list = [
                { id: '10000', parentId: null, name: '账号管理' },
                { id: '11000', parentId: '10000', name: '用户管理' },
                { id: '11100', parentId: '11000', name: '查看' },
                { id: '11110', parentId: '11100', name: '用户列表' },
                { id: '11200', parentId: '11000', name: '编辑' },
                { id: '11210', parentId: '11200', name: '用户列表' },
                { id: '11220', parentId: '11200', name: '新增用户' },
                { id: '11300', parentId: '11000', name: '操作' },
                { id: '11310', parentId: '11300', name: '新增' },
                { id: '11320', parentId: '11300', name: '删除' },
                { id: '11330', parentId: '11300', name: '修改' },
                { id: '12000', parentId: '10000', name: '角色管理' },
                { id: '12100', parentId: '12000', name: '查看' },
                { id: '12110', parentId: '12100', name: '角色列表' },
                { id: '12200', parentId: '12000', name: '编辑' },
                { id: '122100', parentId: '12200', name: '角色列表' },
                { id: '12220', parentId: '12200', name: '新增角色' },
                { id: '12300', parentId: '12000', name: '操作' },
                { id: '12310', parentId: '12300', name: '新增' },
                { id: '12320', parentId: '12300', name: '删除' },
                { id: '12330', parentId: '12300', name: '修改' },
                { id: '20000', parentId: null, name: '个人中心' },
                { id: '21000', parentId: '20000', name: '个性化设置' },
                { id: '21100', parentId: '21000', name: '查看' },
                { id: '21110', parentId: '21100', name: '信息列表' },
                { id: '21200', parentId: '21000', name: '操作' },
                { id: '21210', parentId: '21200', name: '重置信息' },
                { id: '22000', parentId: '20000', name: '账户管理' },
                { id: '22100', parentId: '22000', name: '查看' },
                { id: '22110', parentId: '22100', name: '账户余额' },
                { id: '22120', parentId: '22100', name: '帐变记录' }
              ]
              return list
            }

            // 将普通树结构转换为横向树列表
            const toColTreeData = (treeData: any[]) => {
              const options = { children: 'children' }
              const list: any[] = []
              const keyMap: any = {}
              XEUtils.eachTree(treeData, (item, index, result, paths, parent) => {
                keyMap[item.id] = item
                item.keys = parent ? parent.keys.concat([item.id]) : [item.id]
                if (!item.children || !item.children.length) {
                  const row: any = {}
                  item.keys.forEach((key: any, index: number) => {
                    const level = index + 1
                    const obj = keyMap[key]
                    row[\`check\${level}\`] = false
                    row[\`id\${level}\`] = obj.id
                    row[\`name\${level}\`] = obj.name
                  })
                  list.push(row)
                }
              }, options)
              demo1.tableData = list
            }

            // 通用行合并函数（将相同多列数据合并为一行）
            const rowspanMethod: VxeTablePropTypes.SpanMethod = ({ row, _rowIndex, column, visibleData }) => {
              const fields = ['name1', 'name2', 'name3']
              const cellValue = row[column.property]
              if (cellValue && fields.includes(column.property)) {
                const prevRow = visibleData[_rowIndex - 1]
                let nextRow = visibleData[_rowIndex + 1]
                if (prevRow && prevRow[column.property] === cellValue) {
                  return { rowspan: 0, colspan: 0 }
                } else {
                  let countRowspan = 1
                  while (nextRow && nextRow[column.property] === cellValue) {
                    nextRow = visibleData[++countRowspan + _rowIndex]
                  }
                  if (countRowspan > 1) {
                    return { rowspan: countRowspan, colspan: 1 }
                  }
                }
              }
            }

            const treeData = XEUtils.toArrayTree(getList())
            toColTreeData(treeData)

            return {
              demo1,
              check1ChangeEvent,
              check2ChangeEvent,
              check3ChangeEvent,
              check4ChangeEvent,
              rowspanMethod
            }
          }
        })
        `
      ]
    }
  }
})
</script>
