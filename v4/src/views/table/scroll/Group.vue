<template>
  <div>
    <p class="tip">分组表头<br><span class="red">（注：分组表头不支持横向虚拟滚动，通过设置 scroll-x={enabled: false} 关闭即可）</span></p>

    <vxe-table
      border
      resizable
      show-overflow
      ref="xTable"
      height="500"
      :scroll-x="{enabled: false}"
      :loading="demo1.loading">
      <vxe-column type="seq" title="序号" width="100"></vxe-column>
      <vxe-colgroup title="基本信息">
        <vxe-column field="name" title="Name" width="200" sortable></vxe-column>
        <vxe-column field="age" title="Age" width="200"></vxe-column>
        <vxe-column field="sex" title="Sex" width="200"></vxe-column>
      </vxe-colgroup>
      <vxe-colgroup title="详细信息">
        <vxe-colgroup title="分组">
          <vxe-column field="rate" title="Rate" width="200"></vxe-column>
          <vxe-column field="region" title="Region" width="200"></vxe-column>
        </vxe-colgroup>
        <vxe-colgroup title="其他">
          <vxe-column field="time" title="Time" width="200" sortable></vxe-column>
          <vxe-column field="address" title="Address" width="300" show-overflow></vxe-column>
        </vxe-colgroup>
      </vxe-colgroup>
      <vxe-column field="updateTime" title="UpdateTime" width="200"></vxe-column>
      <vxe-column field="createTime" title="CreateTime" width="200"></vxe-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="xml">{{ demoCodes[0] }}</pre-code>
      <pre-code class="typescript">{{ demoCodes[1] }}</pre-code>
    </pre>
  </div>
</template>

<script lang="tsx">
import { defineComponent, reactive, ref, onMounted, nextTick } from 'vue'
import { VxeTableInstance } from 'vxe-table'

export default defineComponent({
  setup () {
    const mockList = (size: number): Promise<any[]> => {
      return new Promise(resolve => {
        const list: any[] = []
        for (let index = 0; index < size; index++) {
          list.push({
            name: `名称${index}`,
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
    }

    const xTable = ref({} as VxeTableInstance)

    const demo1 = reactive({
      loading: false
    })

    demo1.loading = true
    onMounted(() => {
      mockList(1000).then(data => {
        demo1.loading = false
        nextTick(() => {
          const $table = xTable.value
          if ($table) {
            $table.loadData(data)
          }
        })
      })
    })

    return {
      demo1,
      xTable,
      demoCodes: [
        `
        <vxe-table
          border
          resizable
          show-overflow
          ref="xTable"
          height="500"
          :scroll-x="{enabled: false}"
          :loading="demo1.loading">
          <vxe-column type="seq" title="序号" width="100"></vxe-column>
          <vxe-colgroup title="基本信息">
            <vxe-column field="name" title="Name" width="200" sortable></vxe-column>
            <vxe-column field="age" title="Age" width="200"></vxe-column>
            <vxe-column field="sex" title="Sex" width="200"></vxe-column>
          </vxe-colgroup>
          <vxe-colgroup title="详细信息">
            <vxe-colgroup title="分组">
              <vxe-column field="rate" title="Rate" width="200"></vxe-column>
              <vxe-column field="region" title="Region" width="200"></vxe-column>
            </vxe-colgroup>
            <vxe-colgroup title="其他">
              <vxe-column field="time" title="Time" width="200" sortable></vxe-column>
              <vxe-column field="address" title="Address" width="300" show-overflow></vxe-column>
            </vxe-colgroup>
          </vxe-colgroup>
          <vxe-column field="updateTime" title="UpdateTime" width="200"></vxe-column>
          <vxe-column field="createTime" title="CreateTime" width="200"></vxe-column>
        </vxe-table>
        `,
        `
        import { defineComponent, reactive, ref, onMounted, nextTick } from 'vue'
        import { VxeTableInstance } from 'vxe-table'

        export default defineComponent({
          setup () {
            const mockList = (size: number): Promise<any[]> => {
              return new Promise(resolve => {
                const list: any[] = []
                for (let index = 0; index < size; index++) {
                  list.push({
                    name: \`名称\${index}\`,
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
            }

            const xTable = ref({} as VxeTableInstance)

            const demo1 = reactive({
              loading: false
            })

            demo1.loading = true
            onMounted(() => {
              mockList(1000).then(data => {
                demo1.loading = false
                nextTick(() => {
                  const $table = xTable.value
                  if ($table) {
                    $table.loadData(data)
                  }
                })
              })
            })

            return {
              demo1,
              xTable
            }
          }
        })
        `
      ]
    }
  }
})
</script>
