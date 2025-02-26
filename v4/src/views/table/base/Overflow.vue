<template>
  <div>
    <p class="tip">
      当内容溢出时显示为省略号，<table-column-api-link prop="show-overflow"/> 和 <table-api-link prop="show-header-overflow"/> 和 <table-api-link prop="show-footer-overflow"/><br>
      <table-column-api-link prop="ellipsis"/> 当内容溢出时显示为省略号<br>
      <table-column-api-link prop="title"/> 当内容溢出时显示为省略号并用原生 title 显示<br>
      <table-column-api-link prop="tooltip"/> 当内容溢出时显示为省略号并用 tooltip 显示<br>
    </p>

    <vxe-table
      show-footer
      highlight-hover-row
      :footer-method="footerMethod"
      :data="demo1.tableData1">
      <vxe-column type="seq" width="60"></vxe-column>
      <vxe-column field="name" title="名称" show-overflow="ellipsis"></vxe-column>
      <vxe-column field="role" title="角色" show-overflow></vxe-column>
      <vxe-column field="date" title="标题溢出，显示为 tooltip xxxxxxxxxx" show-header-overflow show-overflow="title" show-footer-overflow></vxe-column>
      <vxe-column field="rate" title="Rate" show-header-overflow="title">
        <template #header>
          <span>标题显示原生 title ___________________________</span>
        </template>
      </vxe-column>
      <vxe-column field="address" title="不换行不换行不换行不换行不换行不换行不换行不换行不换行" width="160"></vxe-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="xml">{{ demoCodes[0] }}</pre-code>
      <pre-code class="typescript">{{ demoCodes[1] }}</pre-code>
    </pre>

    <p class="tip">
      通过设置 <table-api-link prop="contentMethod"/> 方法重写默认的提示内容，显示逻辑完全自定义控制，可以返回 null 使用默认的提示消息<br>
      还可以通过 <table-api-link prop="enterable"/> 开启鼠标是否可进入到 tooltip 中
    </p>

    <vxe-table
      border
      show-footer
      show-header-overflow
      highlight-hover-row
      height="500"
      :footer-method="footerMethod"
      :data="demo2.tableData2"
      :tooltip-config="demo2.tableTooltipConfig">
      <vxe-column type="seq" width="60"></vxe-column>
      <vxe-column field="name" title="名称"></vxe-column>
      <vxe-column field="role" title="标题溢出直接隐藏 xxxxxxxxxxxxxxxxxxxxxxxxxxx" show-header-overflow="ellipsis"></vxe-column>
      <vxe-column field="date" title="Date" show-header-overflow show-overflow show-footer-overflow></vxe-column>
      <vxe-colgroup title="基本信息">
        <vxe-colgroup field="rate" show-header-overflow="title">
          <template #header>
            <span>标题溢出显示原生 title xxxxxxxxxxxxxxxxxxxxxxxxxxx</span>
          </template>
        </vxe-colgroup>
        <vxe-colgroup title="详细信息">
          <vxe-column type="html" field="address" title="标题溢出显示tooltip内容内容内容内容内容内容" width="160" show-header-overflow show-overflow></vxe-column>
        </vxe-colgroup>
      </vxe-colgroup>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="xml">{{ demoCodes[2] }}</pre-code>
      <pre-code class="typescript">{{ demoCodes[3] }}</pre-code>
    </pre>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue'
import { VxeTablePropTypes } from 'vxe-table'

export default defineComponent({
  setup () {
    const demo1 = reactive({
      tableData1: [
        { name: 'Test1', role: '前端', date: '内容显示原生 title', rate: 5, address: 'address1' },
        { name: '内容超出隐藏，不显示提示信息xxxxxxxxxxxxxxxxxxx', role: '后端', date: '2020-02-22', rate: 2, address: 'address2\ntooltip文本换行\n换行xx' },
        { name: 'Test3', role: '内容超出一行显示为 tooltip xxxxxxxxxxxxxx', date: '2020-01-01', rate: 0, address: 'address3\ntooltip文本换行\n换行xx' },
        { name: 'Test4', role: '设计师', date: '2020-02-23', rate: 1, address: 'address4' },
        { name: 'Test5', role: '前端', date: '2020-01-20', rate: 3, address: 'address5\ntooltip文本换行\n换行xx' }
      ]
    })

    const demo2 = reactive({
      tableData2: [
        { name: 'Test1', role: '前端', date: '不管是否超出都显示 tooltip', rate: 5, address: '支持多行超出显示 tooltip xxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
        { name: 'Test2', role: '后端', date: '2020-02-22', rate: 2, address: 'address2' },
        { name: 'Test3', role: '前端', date: '2020-01-01', rate: 0, address: '支持多行超出显示 tooltip xxxxxxxxxxxxxxx<br>html换行<br>支持多行超出显示 tooltip xxxxxxxxxxxxxxx' },
        { name: 'Test4', role: '设计师', date: '2020-02-23', rate: 1, address: 'address4<br>html换行<br>换行xx' },
        { name: 'Test5', role: '前端', date: '2020-01-20', rate: 3, address: 'address5 xxxxxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
        { name: 'Test6', role: '后端', date: '2021-01-20', rate: 3, address: 'address6 xxx<br>html换行<br>换行xx' },
        { name: 'Test7', role: '设计师', date: '2020-08-17', rate: 3, address: 'address7 xxxxxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
        { name: 'Test8', role: '前端', date: '2022-01-20', rate: 3, address: 'address8 xxxxxxxx<br>html换行<br>换行xxxxxxxxxxxxxxx' },
        { name: 'Test9', role: '后端', date: '2021-12-15', rate: 3, address: 'address9 xxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
        { name: 'Test10', role: '前端', date: '2020-09-20', rate: 3, address: 'address10 xxxxxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
        { name: 'Test11', role: '后端', date: '2023-06-01', rate: 3, address: 'address11 xxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
        { name: 'Test12', role: '设计师', date: '2020-07-13', rate: 3, address: 'address12 xxx<br>html换行<br>换行xxzzzzzzzzzzzzzzz' }
      ],
      tableTooltipConfig: {
        contentMethod: ({ type, column, row, items, _columnIndex }) => {
          // 重写默认的提示内容
          if (column.property === 'date') {
            if (type === 'header') {
              return '自定义标题提示内容：' + column.title
            } else if (type === 'footer') {
              return '自定义表尾提示内容：' + items[_columnIndex]
            }
            return '自定义提示内容：' + row[column.property]
          }
        },
        enterable: true
      } as VxeTablePropTypes.TooltipConfig
    })

    const footerMethod: VxeTablePropTypes.FooterMethod = ({ columns }) => {
      const footerData = [
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '合计'
          }
          if (['date'].includes(column.property)) {
            return '说明 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
          }
          if (['rate'].includes(column.property)) {
            return '不想换行不想换行不想换行不想换行不想换行不想换行不想换行不想换行'
          }
          return null
        })
      ]
      return footerData
    }

    return {
      demo1,
      demo2,
      footerMethod,
      demoCodes: [
        `
        <vxe-table
          show-footer
          highlight-hover-row
          :footer-method="footerMethod"
          :data="demo1.tableData1">
          <vxe-column type="seq" width="60"></vxe-column>
          <vxe-column field="name" title="名称" show-overflow="ellipsis"></vxe-column>
          <vxe-column field="role" title="角色" show-overflow></vxe-column>
          <vxe-column field="date" title="标题溢出，显示为 tooltip xxxxxxxxxx" show-header-overflow show-overflow="title" show-footer-overflow></vxe-column>
          <vxe-column field="rate" title="Rate" show-header-overflow="title">
            <template #header>
              <span>标题显示原生 title ___________________________</span>
            </template>
          </vxe-column>
          <vxe-column field="address" title="不换行不换行不换行不换行不换行不换行不换行不换行不换行" width="160"></vxe-column>
        </vxe-table>
        `,
        `
        import { defineComponent, reactive } from 'vue'
        import { VxeTablePropTypes } from 'vxe-table'

        export default defineComponent({
          setup () {
            const demo1 = reactive({
              tableData1: [
                { name: 'Test1', role: '前端', date: '内容显示原生 title', rate: 5, address: 'address1' },
                { name: '内容超出隐藏，不显示提示信息xxxxxxxxxxxxxxxxxxx', role: '后端', date: '2020-02-22', rate: 2, address: 'address2\\ntooltip文本换行\\n换行xx' },
                { name: 'Test3', role: '内容超出一行显示为 tooltip xxxxxxxxxxxxxx', date: '2020-01-01', rate: 0, address: 'address3\\ntooltip文本换行\\n换行xx' },
                { name: 'Test4', role: '设计师', date: '2020-02-23', rate: 1, address: 'address4' },
                { name: 'Test5', role: '前端', date: '2020-01-20', rate: 3, address: 'address5\\ntooltip文本换行\\n换行xx' }
              ]
            })

            const footerMethod: VxeTablePropTypes.FooterMethod = ({ columns }) => {
              const footerData = [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '合计'
                  }
                  if (['date'].includes(column.property)) {
                    return '说明 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
                  }
                  if (['rate'].includes(column.property)) {
                    return '不想换行不想换行不想换行不想换行不想换行不想换行不想换行不想换行'
                  }
                  return null
                })
              ]
              return footerData
            }

            return {
              demo1,
              footerMethod
            }
          }
        })
        `,
        `
        <vxe-table
          border
          show-footer
          show-header-overflow
          highlight-hover-row
          height="500"
          :footer-method="footerMethod"
          :data="demo2.tableData2"
          :tooltip-config="demo2.tableTooltipConfig">
          <vxe-column type="seq" width="60"></vxe-column>
          <vxe-column field="name" title="名称"></vxe-column>
          <vxe-column field="role" title="标题溢出直接隐藏 xxxxxxxxxxxxxxxxxxxxxxxxxxx" show-header-overflow="ellipsis"></vxe-column>
          <vxe-column field="date" title="Date" show-header-overflow show-overflow show-footer-overflow></vxe-column>
          <vxe-colgroup title="基本信息">
            <vxe-colgroup field="rate" show-header-overflow="title">
              <template #header>
                <span>标题溢出显示原生 title xxxxxxxxxxxxxxxxxxxxxxxxxxx</span>
              </template>
            </vxe-colgroup>
            <vxe-colgroup title="详细信息">
              <vxe-column type="html" field="address" title="标题溢出显示tooltip内容内容内容内容内容内容" width="160" show-header-overflow show-overflow></vxe-column>
            </vxe-colgroup>
          </vxe-colgroup>
        </vxe-table>
        `,
        `
        import { defineComponent, reactive } from 'vue'
        import { VxeTablePropTypes } from 'vxe-table'

        export default defineComponent({
          setup () {
            const demo2 = reactive({
              tableData2: [
                { name: 'Test1', role: '前端', date: '不管是否超出都显示 tooltip', rate: 5, address: '支持多行超出显示 tooltip xxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
                { name: 'Test2', role: '后端', date: '2020-02-22', rate: 2, address: 'address2' },
                { name: 'Test3', role: '前端', date: '2020-01-01', rate: 0, address: '支持多行超出显示 tooltip xxxxxxxxxxxxxxx<br>html换行<br>支持多行超出显示 tooltip xxxxxxxxxxxxxxx' },
                { name: 'Test4', role: '设计师', date: '2020-02-23', rate: 1, address: 'address4<br>html换行<br>换行xx' },
                { name: 'Test5', role: '前端', date: '2020-01-20', rate: 3, address: 'address5 xxxxxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
                { name: 'Test6', role: '后端', date: '2021-01-20', rate: 3, address: 'address6 xxx<br>html换行<br>换行xx' },
                { name: 'Test7', role: '设计师', date: '2020-08-17', rate: 3, address: 'address7 xxxxxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
                { name: 'Test8', role: '前端', date: '2022-01-20', rate: 3, address: 'address8 xxxxxxxx<br>html换行<br>换行xxxxxxxxxxxxxxx' },
                { name: 'Test9', role: '后端', date: '2021-12-15', rate: 3, address: 'address9 xxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
                { name: 'Test10', role: '前端', date: '2020-09-20', rate: 3, address: 'address10 xxxxxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
                { name: 'Test11', role: '后端', date: '2023-06-01', rate: 3, address: 'address11 xxxxxxxxxxxxxxxxxxx<br>html换行<br>换行xx' },
                { name: 'Test12', role: '设计师', date: '2020-07-13', rate: 3, address: 'address12 xxx<br>html换行<br>换行xxzzzzzzzzzzzzzzz' }
              ],
              tableTooltipConfig: {
                contentMethod: ({ type, column, row, items, _columnIndex }) => {
                  // 重写默认的提示内容
                  if (column.property === 'date') {
                    if (type === 'header') {
                      return '自定义标题提示内容：' + column.title
                    } else if (type === 'footer') {
                      return '自定义表尾提示内容：' + items[_columnIndex]
                    }
                    return '自定义提示内容：' + row[column.property]
                  }
                },
                enterable: true
              } as VxeTablePropTypes.TooltipConfig
            })

            const footerMethod: VxeTablePropTypes.FooterMethod = ({ columns }) => {
              const footerData = [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '合计'
                  }
                  if (['date'].includes(column.property)) {
                    return '说明 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
                  }
                  if (['rate'].includes(column.property)) {
                    return '不想换行不想换行不想换行不想换行不想换行不想换行不想换行不想换行'
                  }
                  return null
                })
              ]
              return footerData
            }

            return {
              demo2,
              footerMethod
            }
          }
        })
        `
      ]
    }
  }
})
</script>
