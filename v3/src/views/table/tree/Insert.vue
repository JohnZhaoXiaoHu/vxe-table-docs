<template>
  <div>
    <p class="tip">插入数据，简单的实现示例</p>

    <vxe-toolbar custom>
      <template #buttons>
        <vxe-button @click="insertEvent">新增</vxe-button>
        <vxe-button @click="$refs.xTable.removeCheckboxRow()">删除选中</vxe-button>
        <vxe-button @click="getInsertEvent">获取新增</vxe-button>
        <vxe-button @click="getRemoveEvent">获取删除</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      resizable
      show-overflow
      keep-source
      ref="xTable"
      :tree-config="treeConfig"
      :edit-config="{trigger: 'click', mode: 'row', showStatus: true}"
      :data="tableData">
      <vxe-column type="checkbox" width="120" tree-node></vxe-column>
      <vxe-column field="name" title="Name" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.name" type="text"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="size" title="Size" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.size" type="text"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="type" title="Type" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.type" type="text"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="date" title="Date" :edit-render="{}">
        <template #edit="{ row }">
          <vxe-input v-model="row.date" type="date" transfer></vxe-input>
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
      tableData: [
        { id: 10000, parentId: null, name: 'vxe-table test abc1', type: 'mp3', size: 1024, date: '2020-08-01' },
        { id: 10050, parentId: null, name: 'Test2', type: 'mp4', size: null, date: '2021-04-01' },
        { id: 24300, parentId: 10050, name: 'Test3', type: 'avi', size: 1024, date: '2020-03-01' },
        { id: 20045, parentId: 24300, name: 'vxe-table test abc4', type: 'html', size: 600, date: '2021-04-01' },
        { id: 10053, parentId: 24300, name: 'vxe-table test abc96', type: 'avi', size: null, date: '2021-04-01' },
        { id: 24330, parentId: 10053, name: 'vxe-table test abc5', type: 'txt', size: 25, date: '2021-10-01' },
        { id: 21011, parentId: 10053, name: 'Test6', type: 'pdf', size: 512, date: '2020-01-01' },
        { id: 22200, parentId: 10053, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23666, parentId: null, name: 'Test8', type: 'xlsx', size: 2048, date: '2020-11-01' },
        { id: 23677, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23671, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23672, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23688, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23681, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23682, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 24555, parentId: null, name: 'vxe-table test abc9', type: 'avi', size: 224, date: '2020-10-01' },
        { id: 24566, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 24577, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' }
      ],
      treeConfig: {
        transform: true,
        rowField: 'id',
        parentField: 'parentId'
      },
      demoCodes: [
        `
        <vxe-toolbar custom>
          <template #buttons>
            <vxe-button @click="insertEvent">新增</vxe-button>
            <vxe-button @click="$refs.xTable.removeCheckboxRow()">删除选中</vxe-button>
            <vxe-button @click="getInsertEvent">获取新增</vxe-button>
            <vxe-button @click="getRemoveEvent">获取删除</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          resizable
          show-overflow
          keep-source
          ref="xTable"
          :tree-config="treeConfig"
          :edit-config="{trigger: 'click', mode: 'row', showStatus: true}"
          :data="tableData">
          <vxe-column type="checkbox" width="120" tree-node></vxe-column>
          <vxe-column field="name" title="Name" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.name" type="text"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="size" title="Size" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.size" type="text"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="type" title="Type" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.type" type="text"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="date" title="Date" :edit-render="{}">
            <template #edit="{ row }">
              <vxe-input v-model="row.date" type="date" transfer></vxe-input>
            </template>
          </vxe-column>
        </vxe-table>
        `,
        `
        import XEUtils from 'xe-utils'
        
        export default {
          data () {
            return {
              tableData: [
                { id: 10000, parentId: null, name: 'vxe-table test abc1', type: 'mp3', size: 1024, date: '2020-08-01' },
                { id: 10050, parentId: null, name: 'Test2', type: 'mp4', size: null, date: '2021-04-01' },
                { id: 24300, parentId: 10050, name: 'Test3', type: 'avi', size: 1024, date: '2020-03-01' },
                { id: 20045, parentId: 24300, name: 'vxe-table test abc4', type: 'html', size: 600, date: '2021-04-01' },
                { id: 10053, parentId: 24300, name: 'vxe-table test abc96', type: 'avi', size: null, date: '2021-04-01' },
                { id: 24330, parentId: 10053, name: 'vxe-table test abc5', type: 'txt', size: 25, date: '2021-10-01' },
                { id: 21011, parentId: 10053, name: 'Test6', type: 'pdf', size: 512, date: '2020-01-01' },
                { id: 22200, parentId: 10053, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23666, parentId: null, name: 'Test8', type: 'xlsx', size: 2048, date: '2020-11-01' },
                { id: 23677, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23671, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23672, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23688, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23681, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23682, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 24555, parentId: null, name: 'vxe-table test abc9', type: 'avi', size: 224, date: '2020-10-01' },
                { id: 24566, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 24577, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' }
              ],
              treeConfig: {
                transform: true,
                rowField: 'id',
                parentField: 'parentId'
              }
            }
          },
          methods: {
            insertEvent () {
              const $table = this.$refs.xTable
              const newRow = {
                name: '新数据',
                date: XEUtils.toDateString(new Date(), 'yyyy-MM-dd')
              }
              $table.insert(newRow).then(({ row }) => $table.setActiveRow(row))
            },
            getInsertEvent () {
              const $table = this.$refs.xTable
              const insertRecords = $table.getInsertRecords()
              this.$XModal.alert(insertRecords.length)
            },
            getRemoveEvent () {
              const $table = this.$refs.xTable
              const removeRecords = $table.getRemoveRecords()
              this.$XModal.alert(removeRecords.length)
            }
          }
        }
        `
      ]
    }
  },
  methods: {
    insertEvent () {
      const $table = this.$refs.xTable
      const newRow = {
        name: '新数据',
        date: XEUtils.toDateString(new Date(), 'yyyy-MM-dd')
      }
      $table.insert(newRow).then(({ row }) => $table.setActiveRow(row))
    },
    getInsertEvent () {
      const $table = this.$refs.xTable
      const insertRecords = $table.getInsertRecords()
      this.$XModal.alert(insertRecords.length)
    },
    getRemoveEvent () {
      const $table = this.$refs.xTable
      const removeRecords = $table.getRemoveRecords()
      this.$XModal.alert(removeRecords.length)
    }
  }
}
</script>
