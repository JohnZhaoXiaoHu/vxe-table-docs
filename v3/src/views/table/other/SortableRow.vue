<template>
  <div>
    <p class="tip">
      表格不支持拖动功能；例如 <table-api-link name="vxe-table"/> 简单示例： <a class="link" href="https://www.npmjs.com/package/sortablejs" target="_blank">sortablejs</a> 等库实现行拖动<br>
      由于直接操作了 Dom 节点，需要与 Vue 的数据同步，必须设置 <table-api-link prop="row-key"/>，并且自行根据 vue 的规则自行实现数据同步<br>
      <span class="red">（注：该示例仅供参考，具体请自行实现）</span>
    </p>

    <vxe-table
      border
      row-key
      ref="xTable1"
      class="sortable-row-demo"
      :scroll-y="{enabled: false}"
      :data="tableData">
      <vxe-column width="60">
        <template #default>
          <span class="drag-btn">
            <i class="vxe-icon--menu"></i>
          </span>
        </template>
        <template #header>
          <vxe-tooltip v-model="showHelpTip1" content="按住后可以上下拖动排序！" enterable>
            <i class="vxe-icon--question" @click="showHelpTip1 = !showHelpTip1"></i>
          </vxe-tooltip>
        </template>
      </vxe-column>
      <vxe-column field="name" title="Name"></vxe-column>
      <vxe-column field="sex" title="Sex"></vxe-column>
      <vxe-column field="age" title="Age"></vxe-column>
      <vxe-column field="address" title="Address" show-overflow></vxe-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="xml">{{ demoCodes[0] }}</pre-code>
      <pre-code class="javascript">{{ demoCodes[1] }}</pre-code>
      <pre-code class="css">{{ demoCodes[2] }}</pre-code>
    </pre>

    <p class="tip">
      例如 <table-api-link name="vxe-grid"/> 简单示例：<br>
      <span class="red">（注：该示例仅供参考，具体请自行实现）</span>
    </p>

    <vxe-grid
      border
      row-key
      ref="xTable2"
      class="sortable-tree-demo"
      :scroll-y="{enabled: false}"
      :columns="tableColumn"
      :data="tableTreeData"
      :tree-config="{children: 'children'}"></vxe-grid>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="xml">{{ demoCodes[3] }}</pre-code>
      <pre-code class="javascript">{{ demoCodes[4] }}</pre-code>
      <pre-code class="css">{{ demoCodes[5] }}</pre-code>
    </pre>
  </div>
</template>

<script>
import Sortable from 'sortablejs'
import XEUtils from 'xe-utils'

export default {
  data () {
    return {
      showHelpTip1: false,
      showHelpTip2: false,
      tableData: [
        { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: 'Man', age: 28, address: 'Shenzhen' },
        { id: 10002, name: 'Test2', nickname: 'T2', role: 'Test', sex: 'Women', age: 22, address: 'Guangzhou' },
        { id: 10003, name: 'Test3', nickname: 'T3', role: 'PM', sex: 'Man', age: 32, address: 'Shanghai' },
        { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: 'Women', age: 23, address: 'Shenzhen' },
        { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: 'Women', age: 30, address: 'Shanghai' }
      ],
      tableColumn: [
        {
          width: 60,
          slots: {
            default: () => {
              return [
                <span class="drag-btn">
                  <i class="vxe-icon--menu"></i>
                </span>
              ]
            },
            header: () => {
              return [
                <vxe-tooltip v-model={ this.showHelpTip2 } content="按住后可以上下拖动排序！" enterable>
                  <i class="vxe-icon--question" onClick={ () => { this.showHelpTip2 = !this.showHelpTip2 } }></i>
                </vxe-tooltip>
              ]
            }
          }
        },
        { field: 'name', title: 'Name', treeNode: true },
        { field: 'size', title: 'Size' },
        { field: 'type', title: 'Type' },
        { field: 'date', title: 'Date' }
      ],
      tableTreeData: [
        { id: 1000, name: 'test abc1', type: 'mp3', size: 1024, date: '2020-08-01' },
        {
          id: 1005,
          name: 'Test2',
          type: 'mp4',
          size: null,
          date: '2021-04-01',
          children: [
            { id: 24300, name: 'Test3', type: 'avi', size: 1024, date: '2020-03-01' },
            { id: 20045, name: 'test abc4', type: 'html', size: 600, date: '2021-04-01' },
            {
              id: 10053,
              name: 'test abc96',
              type: 'avi',
              size: null,
              date: '2021-04-01',
              children: [
                { id: 24330, name: 'test abc5', type: 'txt', size: 25, date: '2021-10-01' },
                { id: 21011, name: 'Test6', type: 'pdf', size: 512, date: '2020-01-01' },
                { id: 22200, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' }
              ]
            }
          ]
        },
        { id: 23666, name: 'Test8', type: 'xlsx', size: 2048, date: '2020-11-01' },
        { id: 24555, name: 'test abc9', type: 'avi', size: 224, date: '2020-10-01' }
      ],
      demoCodes: [
        `
        <vxe-table
          border
          row-key
          ref="xTable1"
          class="sortable-row-demo"
          :scroll-y="{enabled: false}"
          :data="tableData">
          <vxe-column width="60">
            <template #default>
              <span class="drag-btn">
                <i class="vxe-icon--menu"></i>
              </span>
            </template>
            <template #header>
              <vxe-tooltip v-model="showHelpTip1" content="按住后可以上下拖动排序！" enterable>
                <i class="vxe-icon--question" @click="showHelpTip1 = !showHelpTip1"></i>
              </vxe-tooltip>
            </template>
          </vxe-column>
          <vxe-column field="name" title="Name"></vxe-column>
          <vxe-column field="sex" title="Sex"></vxe-column>
          <vxe-column field="age" title="Age"></vxe-column>
          <vxe-column field="address" title="Address" show-overflow></vxe-column>
        </vxe-table>
        `,
        `
        import Sortable from 'sortablejs'
        import XEUtils from 'xe-utils'

        export default {
          data () {
            return {
              showHelpTip1: false,
              tableData: [
                { id: 10001, name: 'Test1', nickname: 'T1', role: 'Develop', sex: 'Man', age: 28, address: 'Shenzhen' },
                { id: 10002, name: 'Test2', nickname: 'T2', role: 'Test', sex: 'Women', age: 22, address: 'Guangzhou' },
                { id: 10003, name: 'Test3', nickname: 'T3', role: 'PM', sex: 'Man', age: 32, address: 'Shanghai' },
                { id: 10004, name: 'Test4', nickname: 'T4', role: 'Designer', sex: 'Women', age: 23, address: 'Shenzhen' },
                { id: 10005, name: 'Test5', nickname: 'T5', role: 'Develop', sex: 'Women', age: 30, address: 'Shanghai' }
              ]
            }
          },
          created () {
            this.rowDrop()
          },
          beforeDestroy () {
            if (this.sortable1) {
              this.sortable1.destroy()
            }
          },
          methods: {
            rowDrop () {
              this.$nextTick(() => {
                const xTable = this.$refs.xTable1
                this.sortable1 = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--body tbody'), {
                  handle: '.drag-btn',
                  onEnd: ({ newIndex, oldIndex }) => {
                    const currRow = this.tableData.splice(oldIndex, 1)[0]
                    this.tableData.splice(newIndex, 0, currRow)
                  }
                })
              })
            }
          }
        }
        `,
        `
        .sortable-row-demo .drag-btn {
          cursor: move;
          font-size: 12px;
        }
        .sortable-row-demo .vxe-body--row.sortable-ghost,
        .sortable-row-demo .vxe-body--row.sortable-chosen {
          background-color: #dfecfb;
        }
        `,
        `
        <vxe-grid
          border
          row-key
          ref="xTable2"
          class="sortable-tree-demo"
          :scroll-y="{enabled: false}"
          :columns="tableColumn"
          :data="tableTreeData"
          :tree-config="{children: 'children'}"></vxe-grid>
        `,
        `
        import Sortable from 'sortablejs'
        import XEUtils from 'xe-utils'

        export default {
          data () {
            return {
              showHelpTip2: false,
              tableTreeData: [
                { id: 1000, name: 'test abc1', type: 'mp3', size: 1024, date: '2020-08-01' },
                {
                  id: 1005,
                  name: 'Test2',
                  type: 'mp4',
                  size: null,
                  date: '2021-04-01',
                  children: [
                    { id: 24300, name: 'Test3', type: 'avi', size: 1024, date: '2020-03-01' },
                    { id: 20045, name: 'test abc4', type: 'html', size: 600, date: '2021-04-01' },
                    {
                      id: 10053,
                      name: 'test abc96',
                      type: 'avi',
                      size: null,
                      date: '2021-04-01',
                      children: [
                        { id: 24330, name: 'test abc5', type: 'txt', size: 25, date: '2021-10-01' },
                        { id: 21011, name: 'Test6', type: 'pdf', size: 512, date: '2020-01-01' },
                        { id: 22200, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' }
                      ]
                    }
                  ]
                },
                { id: 23666, name: 'Test8', type: 'xlsx', size: 2048, date: '2020-11-01' },
                { id: 24555, name: 'test abc9', type: 'avi', size: 224, date: '2020-10-01' }
              ],
              tableColumn: [
                {
                  width: 60,
                  slots: {
                    default: () => {
                      return [
                        <span class="drag-btn">
                          <i class="vxe-icon--menu"></i>
                        </span>
                      ]
                    },
                    header: () => {
                      return [
                        <vxe-tooltip v-model={ this.showHelpTip2 } content="按住后可以上下拖动排序！" enterable>
                          <i class="vxe-icon--question" onClick={ () => { this.showHelpTip2 = !this.showHelpTip2 } }></i>
                        </vxe-tooltip>
                      ]
                    }
                  }
                },
                { field: 'name', title: 'Name', treeNode: true },
                { field: 'size', title: 'Size' },
                { field: 'type', title: 'Type' },
                { field: 'date', title: 'Date' }
              ]
            }
          },
          created () {
            this.treeDrop()
          },
          beforeDestroy () {
            if (this.sortable2) {
              this.sortable2.destroy()
            }
          },
          methods: {
            treeDrop () {
              this.$nextTick(() => {
                const xTable = this.$refs.xTable2
                this.sortable2 = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--body tbody'), {
                  handle: '.drag-btn',
                  onEnd: ({ item, oldIndex }) => {
                    const options = { children: 'children' }
                    const targetTrElem = item
                    const wrapperElem = targetTrElem.parentNode
                    const prevTrElem = targetTrElem.previousElementSibling
                    const tableTreeData = this.tableTreeData
                    const selfRow = xTable.getRowNode(targetTrElem).item
                    const selfNode = XEUtils.findTree(tableTreeData, row => row === selfRow, options)
                    if (prevTrElem) {
                      // 移动到节点
                      const prevRow = xTable.getRowNode(prevTrElem).item
                      const prevNode = XEUtils.findTree(tableTreeData, row => row === prevRow, options)
                      if (XEUtils.findTree(selfRow[options.children], row => prevRow === row, options)) {
                        // 错误的移动
                        const oldTrElem = wrapperElem.children[oldIndex]
                        wrapperElem.insertBefore(targetTrElem, oldTrElem)
                        return this.$XModal.message({ content: '不允许自己给自己拖动！', status: 'error' })
                      }
                      const currRow = selfNode.items.splice(selfNode.index, 1)[0]
                      if (xTable.isTreeExpandByRow(prevRow)) {
                        // 移动到当前的子节点
                        prevRow[options.children].splice(0, 0, currRow)
                      } else {
                        // 移动到相邻节点
                        prevNode.items.splice(prevNode.index + (selfNode.index < prevNode.index ? 0 : 1), 0, currRow)
                      }
                    } else {
                      // 移动到第一行
                      const currRow = selfNode.items.splice(selfNode.index, 1)[0]
                      tableTreeData.unshift(currRow)
                    }
                    // 如果变动了树层级，需要刷新数据
                    this.tableTreeData = [...tableTreeData]
                  }
                })
              })
            }
          }
        }
        `,
        `
        .sortable-tree-demo .drag-btn {
          cursor: move;
          font-size: 12px;
        }
        .sortable-tree-demo .vxe-body--row.sortable-ghost,
        .sortable-tree-demo .vxe-body--row.sortable-chosen {
          background-color: #dfecfb;
        }
        `
      ]
    }
  },
  created () {
    this.rowDrop()
    this.treeDrop()
  },
  beforeDestroy () {
    if (this.sortable1) {
      this.sortable1.destroy()
    }
    if (this.sortable2) {
      this.sortable2.destroy()
    }
  },
  methods: {
    rowDrop () {
      this.$nextTick(() => {
        const xTable = this.$refs.xTable1
        this.sortable1 = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--body tbody'), {
          handle: '.drag-btn',
          onEnd: ({ newIndex, oldIndex }) => {
            const currRow = this.tableData.splice(oldIndex, 1)[0]
            this.tableData.splice(newIndex, 0, currRow)
          }
        })
      })
    },
    treeDrop () {
      this.$nextTick(() => {
        const xTable = this.$refs.xTable2
        this.sortable2 = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--body tbody'), {
          handle: '.drag-btn',
          onEnd: ({ item, oldIndex }) => {
            const options = { children: 'children' }
            const targetTrElem = item
            const wrapperElem = targetTrElem.parentNode
            const prevTrElem = targetTrElem.previousElementSibling
            const tableTreeData = this.tableTreeData
            const selfRow = xTable.getRowNode(targetTrElem).item
            const selfNode = XEUtils.findTree(tableTreeData, row => row === selfRow, options)
            if (prevTrElem) {
              // 移动到节点
              const prevRow = xTable.getRowNode(prevTrElem).item
              const prevNode = XEUtils.findTree(tableTreeData, row => row === prevRow, options)
              if (XEUtils.findTree(selfRow[options.children], row => prevRow === row, options)) {
                // 错误的移动
                const oldTrElem = wrapperElem.children[oldIndex]
                wrapperElem.insertBefore(targetTrElem, oldTrElem)
                return this.$XModal.message({ content: '不允许自己给自己拖动！', status: 'error' })
              }
              const currRow = selfNode.items.splice(selfNode.index, 1)[0]
              if (xTable.isTreeExpandByRow(prevRow)) {
                // 移动到当前的子节点
                prevRow[options.children].splice(0, 0, currRow)
              } else {
                // 移动到相邻节点
                prevNode.items.splice(prevNode.index + (selfNode.index < prevNode.index ? 0 : 1), 0, currRow)
              }
            } else {
              // 移动到第一行
              const currRow = selfNode.items.splice(selfNode.index, 1)[0]
              tableTreeData.unshift(currRow)
            }
            // 如果变动了树层级，需要刷新数据
            this.tableTreeData = [...tableTreeData]
          }
        })
      })
    }
  }
}
</script>

<style lang="scss">
.sortable-row-demo .drag-btn {
  cursor: move;
  font-size: 12px;
}
.sortable-row-demo .vxe-body--row.sortable-ghost,
.sortable-row-demo .vxe-body--row.sortable-chosen {
  background-color: #dfecfb;
}

.sortable-tree-demo .drag-btn {
  cursor: move;
  font-size: 12px;
}
.sortable-tree-demo .vxe-body--row.sortable-ghost,
.sortable-tree-demo .vxe-body--row.sortable-chosen {
  background-color: #dfecfb;
}
</style>
