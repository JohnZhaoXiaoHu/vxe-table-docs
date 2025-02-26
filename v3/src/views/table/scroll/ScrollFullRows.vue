<template>
  <div>
    <p class="tip">
      虚拟滚动渲染，左右固定列<br>
      大数据不建议使用双向绑定的 data 属性（vue 监听会大数据会短暂的卡顿），建议使用 <table-api-link prop="loadData"/>/<table-api-link prop="reloadData"/> 函数<br>
      对于多选 type=<table-column-api-link prop="checkbox"/> 当数据量海量时应该绑定 <table-api-link prop="checkField"/> 属性渲染速度更快<br>
      <span class="red">（注：启用纵向虚拟滚启动后不支持动态行高；如果需要支持，将虚拟滚动关闭即可）</span>
    </p>

    <vxe-toolbar>
      <template #buttons>
        <vxe-button @click="loadList(10000)">1w条</vxe-button>
        <vxe-button @click="loadList(50000)">5w条</vxe-button>
        <vxe-button @click="loadList(100000)">10w条</vxe-button>
        <vxe-button @click="loadList(200000)">20w条</vxe-button>
        <vxe-button @click="$refs.xTable.setAllCheckboxRow(true)">所有选中</vxe-button>
        <vxe-button @click="$refs.xTable.clearCheckboxRow()">清除选中</vxe-button>
        <vxe-button @click="getSelectEvent">获取选中</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      border
      resizable
      show-overflow
      row-key
      show-header-overflow
      highlight-hover-row
      highlight-current-row
      ref="xTable"
      height="600"
      :export-config="{}"
      :loading="loading"
      :checkbox-config="{checkField: 'checked'}">
      <vxe-column type="checkbox" width="60" fixed="left"></vxe-column>
      <vxe-column type="seq" width="100" fixed="left"></vxe-column>
      <vxe-column field="name" title="Name" sortable width="200"></vxe-column>
      <vxe-column field="sex" title="Sex" width="200"></vxe-column>
      <vxe-column field="rate" title="Rate" width="200"></vxe-column>
      <vxe-column field="region" title="Region" width="200"></vxe-column>
      <vxe-column field="time" title="Time" width="200"></vxe-column>
      <vxe-column field="address" title="Address" width="300"></vxe-column>
      <vxe-column field="updateTime" title="UpdateTime" width="200"></vxe-column>
      <vxe-column field="createTime" title="CreateTime" width="200"></vxe-column>
      <vxe-column field="attr1" title="Attr1" width="200"></vxe-column>
      <vxe-column field="attr2" title="Attr2" width="200"></vxe-column>
      <vxe-column field="attr3" title="Attr3" width="200"></vxe-column>
      <vxe-column field="attr4" title="Attr4" width="200"></vxe-column>
      <vxe-column field="attr5" title="Attr5" width="200"></vxe-column>
      <vxe-column field="attr6" title="Attr6" width="200"></vxe-column>
      <vxe-column field="attr7" title="Attr7" width="200"></vxe-column>
      <vxe-column field="attr8" title="Attr8" width="200"></vxe-column>
      <vxe-column field="attr9" title="Attr9" width="200"></vxe-column>
      <vxe-column field="attr10" title="Attr10" width="200"></vxe-column>
      <vxe-column field="age" title="Age" width="200" fixed="right"></vxe-column>
    </vxe-table>

    <pre>
      <pre-code>
        | Arrow Up ↑ | 匀速向上滚动数据 |
        | Arrow Down ↓ | 匀速向下滚动数据 |
        | Arrow Left ← | 匀速向左滚动数据 |
        | Arrow Right → | 匀速向右滚动数据 |
        | Page Up | 向上翻页滚动 |
        | Page Down | 向下翻页滚动 |
        | Spacebar | 翻页滚动 |
        | Home | 滚动到顶部 |
        | End | 滚动到底部 |
      </pre-code>
    </pre>

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
      loading: false,
      demoCodes: [
        `
        <vxe-toolbar>
          <template #buttons>
            <vxe-button @click="loadList(10000)">1w条</vxe-button>
            <vxe-button @click="loadList(50000)">5w条</vxe-button>
            <vxe-button @click="loadList(100000)">10w条</vxe-button>
            <vxe-button @click="loadList(200000)">20w条</vxe-button>
            <vxe-button @click="$refs.xTable.setAllCheckboxRow(true)">所有选中</vxe-button>
            <vxe-button @click="$refs.xTable.clearCheckboxRow()">清除选中</vxe-button>
            <vxe-button @click="getSelectEvent">获取选中</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          border
          resizable
          show-overflow
          row-key
          show-header-overflow
          highlight-hover-row
          highlight-current-row
          ref="xTable"
          height="600"
          :export-config="{}"
          :loading="loading"
          :checkbox-config="{checkField: 'checked'}">
          <vxe-column type="checkbox" width="60" fixed="left"></vxe-column>
          <vxe-column type="seq" width="100" fixed="left"></vxe-column>
          <vxe-column field="name" title="Name" sortable width="200"></vxe-column>
          <vxe-column field="sex" title="Sex" width="200"></vxe-column>
          <vxe-column field="rate" title="Rate" width="200"></vxe-column>
          <vxe-column field="region" title="Region" width="200"></vxe-column>
          <vxe-column field="time" title="Time" width="200"></vxe-column>
          <vxe-column field="address" title="Address" width="300"></vxe-column>
          <vxe-column field="updateTime" title="UpdateTime" width="200"></vxe-column>
          <vxe-column field="createTime" title="CreateTime" width="200"></vxe-column>
          <vxe-column field="attr1" title="Attr1" width="200"></vxe-column>
          <vxe-column field="attr2" title="Attr2" width="200"></vxe-column>
          <vxe-column field="attr3" title="Attr3" width="200"></vxe-column>
          <vxe-column field="attr4" title="Attr4" width="200"></vxe-column>
          <vxe-column field="attr5" title="Attr5" width="200"></vxe-column>
          <vxe-column field="attr6" title="Attr6" width="200"></vxe-column>
          <vxe-column field="attr7" title="Attr7" width="200"></vxe-column>
          <vxe-column field="attr8" title="Attr8" width="200"></vxe-column>
          <vxe-column field="attr9" title="Attr9" width="200"></vxe-column>
          <vxe-column field="attr10" title="Attr10" width="200"></vxe-column>
          <vxe-column field="age" title="Age" width="200" fixed="right"></vxe-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              loading: false
            }
          },
          created () {
            this.loadList(600)
          },
          methods: {
            loadList (size) {
              this.loading = true
              this.mockList(size).then(data => {
                // 使用函数式加载，阻断 vue 对大数据的监听
                const xTable = this.$refs.xTable
                const startTime = Date.now()
                if (xTable) {
                  this.$refs.xTable.reloadData(data).then(() => {
                    this.$XModal.message({ content: \`渲染 \${size} 行，用时 \${Date.now() - startTime}毫秒\`, status: 'info' })
                    this.loading = false
                  })
                }
              })
            },
            mockList (size) {
              return new Promise(resolve => {
                const list = []
                for (let index = 0; index < size; index++) {
                  list.push({
                    name: \`名称\${index}\`,
                    checked: false,
                    sex: '0',
                    num: 123,
                    age: 18,
                    num2: 234,
                    rate: 3,
                    address: 'shenzhen'
                  })
                }
                resolve(list)
              })
            },
            getSelectEvent () {
              let selectRecords = this.$refs.xTable.getCheckboxRecords()
              this.$XModal.alert(selectRecords.length)
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.loadList(600)
  },
  methods: {
    loadList (size) {
      this.loading = true
      this.mockList(size).then(data => {
        // 使用函数式加载，阻断 vue 对大数据的监听
        const xTable = this.$refs.xTable
        const startTime = Date.now()
        if (xTable) {
          this.$refs.xTable.reloadData(data).then(() => {
            this.$XModal.message({ content: `渲染 ${size} 行，用时 ${Date.now() - startTime}毫秒`, status: 'info' })
            this.loading = false
          })
        }
      })
    },
    mockList (size) {
      return new Promise(resolve => {
        const list = []
        for (let index = 0; index < size; index++) {
          list.push({
            name: `名称${index}`,
            checked: false,
            sex: '0',
            num: 123,
            age: 18,
            num2: 234,
            rate: 3,
            address: 'shenzhen'
          })
        }
        resolve(list)
      })
    },
    getSelectEvent () {
      const selectRecords = this.$refs.xTable.getCheckboxRecords()
      this.$XModal.alert(selectRecords.length)
    }
  }
}
</script>
