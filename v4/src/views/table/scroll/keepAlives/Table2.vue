<template>
  <div>
    <p>子路由 2</p>

    <vxe-table
      border
      show-overflow
      ref="xTable"
      height="400"
      :loading="demo1.loading">
      <vxe-column type="radio" width="60"></vxe-column>
      <vxe-column field="role" title="Role"></vxe-column>
      <vxe-column field="age" title="Age"></vxe-column>
      <vxe-column field="num2" title="Num2"></vxe-column>
      <vxe-column field="rate" title="Rate"></vxe-column>
    </vxe-table>
  </div>
</template>

<script lang="tsx">
import { defineComponent, reactive, ref, nextTick } from 'vue'
import { VxeTableInstance } from 'vxe-table'

export default defineComponent({
  setup () {
    const xTable = ref({} as VxeTableInstance)

    const mockList = (size: number) => {
      const list: any[] = []
      for (let index = 0; index < size; index++) {
        list.push({
          name: `名称${index}`,
          role: `角色${index}`,
          sex: '0',
          num: 123,
          age: 18,
          num2: 234,
          rate: 3,
          address: 'shenzhen'
        })
      }
      return list
    }

    const demo1 = reactive({
      loading: false
    })

    demo1.loading = true
    setTimeout(() => {
      demo1.loading = false
      nextTick(() => {
        const $table = xTable.value
        if ($table) {
          $table.loadData(mockList(600))
        }
      })
    }, 300)

    return {
      demo1,
      xTable
    }
  }
})
</script>
