<template>
  <div>
    <p class="tip">
      通过调用 <table-api-link prop="validate"/> 函数校验数据，<table-api-link prop="edit-rules"/> 校验规则配置<br>
      <span class="red">（不指定数据的情况下，默认只校验状态发生变动的数据，例如：新增、修改...等）</span>
    </p>

    <vxe-toolbar>
      <template #buttons>
        <vxe-button @click="validEvent">快速校验</vxe-button>
        <vxe-button @click="fullValidEvent">完整快速校验</vxe-button>
        <vxe-button @click="selectValidEvent">选中行校验</vxe-button>
        <vxe-button @click="getSelectEvent">获取选中</vxe-button>
        <vxe-button @click="getUpdateEvent">获取修改</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      resizable
      show-overflow
      keep-source
      ref="xTable"
      :edit-rules="validRules"
      :tree-config="treeConfig"
      :edit-config="{trigger: 'click', mode: 'cell', showStatus: true}"
      :checkbox-config="{labelField: 'id'}"
      :data="tableData">
      <vxe-column type="checkbox" title="ID" tree-node></vxe-column>
      <vxe-column field="name" title="Name" :edit-render="{}">
        <template #edit="scope">
          <vxe-input v-model="scope.row.name" type="text" @change="$refs.xTable.updateStatus(scope)"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="size" title="Size" :edit-render="{}">
        <template #edit="scope">
          <vxe-input v-model="scope.row.size" type="text" @change="$refs.xTable.updateStatus(scope)"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="type" title="Type" :edit-render="{}">
        <template #edit="scope">
          <vxe-input v-model="scope.row.type" type="text" @change="$refs.xTable.updateStatus(scope)"></vxe-input>
        </template>
      </vxe-column>
      <vxe-column field="date" title="Date" :edit-render="{}">
        <template #edit="scope">
          <vxe-input v-model="scope.row.date" type="date" transfer @change="$refs.xTable.updateStatus(scope)"></vxe-input>
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
        { id: 10000, parentId: null, name: 'test abc1', type: 'mp3', size: 1024, date: '2020-08-01' },
        { id: 10050, parentId: null, name: 'Test2', type: 'mp4', size: null, date: '2021-04-01' },
        { id: 24300, parentId: 10050, name: 'Test3', type: 'avi', size: 1024, date: '2020-03-01' },
        { id: 20045, parentId: 24300, name: 'test abc4', type: 'html', size: 600, date: '2021-04-01' },
        { id: 10053, parentId: 24300, name: 'test abc96', type: 'avi', size: null, date: '2021-04-01' },
        { id: 24330, parentId: 10053, name: 'test abc5', type: 'txt', size: 25, date: '2021-10-01' },
        { id: 21011, parentId: 10053, name: 'Test6', type: 'pdf', size: 512, date: '2020-01-01' },
        { id: 22200, parentId: 10053, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23666, parentId: null, name: 'Test8', type: 'xlsx', size: 2048, date: '2020-11-01' },
        { id: 23677, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23671, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23672, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23688, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23681, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 23682, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 24555, parentId: null, name: 'test abc9', type: 'avi', size: 224, date: '2020-10-01' },
        { id: 24566, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
        { id: 24577, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' }
      ],
      treeConfig: {
        transform: true
      },
      validRules: {
        name: [
          { required: true, message: 'app.body.valid.rName' },
          { min: 3, max: 50, message: '文件名长度在 3 到 50 个字符' }
        ]
      },
      demoCodes: [
        `
        <vxe-toolbar>
          <template #buttons>
            <vxe-button @click="validEvent">快速校验</vxe-button>
            <vxe-button @click="fullValidEvent">完整快速校验</vxe-button>
            <vxe-button @click="selectValidEvent">选中行校验</vxe-button>
            <vxe-button @click="getSelectEvent">获取选中</vxe-button>
            <vxe-button @click="getUpdateEvent">获取修改</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          resizable
          show-overflow
          keep-source
          ref="xTable"
          :edit-rules="validRules"
          :tree-config="treeConfig"
          :edit-config="{trigger: 'click', mode: 'cell', showStatus: true}"
          :checkbox-config="{labelField: 'id'}"
          :data="tableData">
          <vxe-column type="checkbox" title="ID" tree-node></vxe-column>
          <vxe-column field="name" title="Name" :edit-render="{}">
            <template #edit="scope">
              <vxe-input v-model="scope.row.name" type="text" @change="$refs.xTable.updateStatus(scope)"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="size" title="Size" :edit-render="{}">
            <template #edit="scope">
              <vxe-input v-model="scope.row.size" type="text" @change="$refs.xTable.updateStatus(scope)"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="type" title="Type" :edit-render="{}">
            <template #edit="scope">
              <vxe-input v-model="scope.row.type" type="text" @change="$refs.xTable.updateStatus(scope)"></vxe-input>
            </template>
          </vxe-column>
          <vxe-column field="date" title="Date" :edit-render="{}">
            <template #edit="scope">
              <vxe-input v-model="scope.row.date" type="date" transfer @change="$refs.xTable.updateStatus(scope)"></vxe-input>
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
                { id: 10000, parentId: null, name: 'test abc1', type: 'mp3', size: 1024, date: '2020-08-01' },
                { id: 10050, parentId: null, name: 'Test2', type: 'mp4', size: null, date: '2021-04-01' },
                { id: 24300, parentId: 10050, name: 'Test3', type: 'avi', size: 1024, date: '2020-03-01' },
                { id: 20045, parentId: 24300, name: 'test abc4', type: 'html', size: 600, date: '2021-04-01' },
                { id: 10053, parentId: 24300, name: 'test abc96', type: 'avi', size: null, date: '2021-04-01' },
                { id: 24330, parentId: 10053, name: 'test abc5', type: 'txt', size: 25, date: '2021-10-01' },
                { id: 21011, parentId: 10053, name: 'Test6', type: 'pdf', size: 512, date: '2020-01-01' },
                { id: 22200, parentId: 10053, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23666, parentId: null, name: 'Test8', type: 'xlsx', size: 2048, date: '2020-11-01' },
                { id: 23677, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23671, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23672, parentId: 23677, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23688, parentId: 23666, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23681, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 23682, parentId: 23688, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 24555, parentId: null, name: 'test abc9', type: 'avi', size: 224, date: '2020-10-01' },
                { id: 24566, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' },
                { id: 24577, parentId: 24555, name: 'Test7', type: 'js', size: 1024, date: '2021-06-01' }
              ],
              treeConfig: {
                transform: true
              },
              validRules: {
                name: [
                  { required: true, message: 'app.body.valid.rName' },
                  { min: 3, max: 50, message: '文件名长度在 3 到 50 个字符' }
                ]
              }
            }
          },
          methods: {
            async validEvent () {
              const $table = this.$refs.xTable
              const errMap = await $table.validate().catch(errMap => errMap)
              if (errMap) {
                this.$XModal.message({ status: 'error', content: '校验不通过！' })
              } else {
                this.$XModal.message({ status: 'success', content: '校验成功！' })
              }
            },
            async fullValidEvent () {
              const errMap = await this.$refs.xTable.fullValidate()
              if (errMap) {
                const msgList = []
                Object.values(errMap).forEach(errList => {
                  errList.forEach(params => {
                    const { row, column, rules } = params
                    const matchObj = XEUtils.findTree(this.tableData, item => item === row, this.treeConfig)
                    const seq = matchObj.path.filter(item => item !== this.treeConfig.children).map(item => Number(item) + 1).join('.')
                    rules.forEach(rule => {
                      msgList.push(\`第 \${seq} 行 \${column.title} 校验错误：\${rule.message}\`)
                    })
                  })
                })
                this.$XModal.message({
                  status: 'error',
                  slots: {
                    default () {
                      return [
                        <div class="red" style="max-height: 400px;overflow: auto;">
                          {
                            msgList.map(msg => {
                              return <div>{ msg }</div>
                            })
                          }
                        </div>
                      ]
                    }
                  }
                })
              } else {
                this.$XModal.message({ status: 'success', content: '校验成功！' })
              }
            },
            async selectValidEvent () {
              const $table = this.$refs.xTable
              const selectRecords = $table.getCheckboxRecords()
              if (selectRecords.length > 0) {
                const errMap = await $table.validate(selectRecords).catch(errMap => errMap)
                if (errMap) {
                  this.$XModal.message({ status: 'error', content: '校验不通过！' })
                } else {
                  this.$XModal.message({ status: 'success', content: '校验成功！' })
                }
              } else {
                this.$XModal.message({ status: 'warning', content: '未选中数据！' })
              }
            },
            getSelectEvent () {
              let selectRecords = this.$refs.xTable.getCheckboxRecords()
              this.$XModal.alert(selectRecords.length)
            },
            getUpdateEvent () {
              let updateRecords = this.$refs.xTable.getUpdateRecords()
              this.$XModal.alert(updateRecords.length)
            }
          }
        }
        `
      ]
    }
  },
  methods: {
    async validEvent () {
      const $table = this.$refs.xTable
      const errMap = await $table.validate().catch(errMap => errMap)
      if (errMap) {
        this.$XModal.message({ status: 'error', content: '校验不通过！' })
      } else {
        this.$XModal.message({ status: 'success', content: '校验成功！' })
      }
    },
    async fullValidEvent () {
      const errMap = await this.$refs.xTable.fullValidate()
      if (errMap) {
        const msgList = []
        Object.values(errMap).forEach(errList => {
          errList.forEach(params => {
            const { row, column, rules } = params
            const matchObj = XEUtils.findTree(this.tableData, item => item === row, this.treeConfig)
            const seq = matchObj.path.filter(item => item !== this.treeConfig.children).map(item => Number(item) + 1).join('.')
            rules.forEach(rule => {
              msgList.push(`第 ${seq} 行 ${column.title} 校验错误：${rule.message}`)
            })
          })
        })
        this.$XModal.message({
          status: 'error',
          slots: {
            default () {
              return [
                <div class="red" style="max-height: 400px;overflow: auto;">
                  {
                    msgList.map(msg => {
                      return <div>{ msg }</div>
                    })
                  }
                </div>
              ]
            }
          }
        })
      } else {
        this.$XModal.message({ status: 'success', content: '校验成功！' })
      }
    },
    async selectValidEvent () {
      const $table = this.$refs.xTable
      const selectRecords = $table.getCheckboxRecords()
      if (selectRecords.length > 0) {
        const errMap = await $table.validate(selectRecords).catch(errMap => errMap)
        if (errMap) {
          this.$XModal.message({ status: 'error', content: '校验不通过！' })
        } else {
          this.$XModal.message({ status: 'success', content: '校验成功！' })
        }
      } else {
        this.$XModal.message({ status: 'warning', content: '未选中数据！' })
      }
    },
    getSelectEvent () {
      const selectRecords = this.$refs.xTable.getCheckboxRecords()
      this.$XModal.alert(selectRecords.length)
    },
    getUpdateEvent () {
      const updateRecords = this.$refs.xTable.getUpdateRecords()
      this.$XModal.alert(updateRecords.length)
    }
  }
}
</script>
