<template>
  <div>
    <p class="tip">
      表单-项渲染 <grid-api-link prop="itemRender"/>，查看 <a class="link" href="https://github.com/x-extends/vxe-table-docs/tree/main/v4/src/plugins/table/renderer" target="_blank">示例的源码</a><span class="red">（具体请自行实现，该示例仅供参考）</span><br>
      配置参数：<br>
      renderItemTitle (h, renderOpts, params: { data, property, $form }) 表单项标题<br>
      renderItemContent (h, renderOpts, params: { data, property, $form }) 表单项内容<br>
      itemVisibleMethod (params: { data, property }) 表单项可视函数<br>
      itemResetMethod (params: { data, property }) 表单项重置函数<br>
    </p>

    <vxe-grid
      border
      resizable
      height="400"
      :export-config="{}"
      :toolbar-config="{export: true, custom: true}"
      :form-config="tableForm"
      :proxy-config="tableProxy"
      :columns="tableColumn">
    </vxe-grid>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <pre-code class="javascript">{{ demoCodes[0] }}</pre-code>
      <pre-code class="xml">{{ demoCodes[1] }}</pre-code>
      <pre-code class="javascript">{{ demoCodes[2] }}</pre-code>
    </pre>
  </div>
</template>

<script>
export default {
  data () {
    return {
      tableColumn: [
        { type: 'seq', width: 50 },
        { field: 'name', title: 'Name' },
        { field: 'age', title: 'Age' },
        { field: 'sex', title: 'Sex' },
        { field: 'role', title: 'Role' }
      ],
      tableForm: {
        items: [
          { field: 'name', title: '名称', itemRender: { name: 'FormItemInput', props: { clearable: true, placeholder: '请输入名称' } } },
          { field: 'age', title: '年龄', itemRender: { name: 'FormItemInput', props: { type: 'number', clearable: true, placeholder: '请输入年龄' } } },
          { itemRender: { name: 'input', attrs: { type: 'submit', value: '查询' } } },
          { itemRender: { name: 'input', attrs: { type: 'reset', value: '重置' } } }
        ]
      },
      tableProxy: {
        form: true,
        ajax: {
          query: () => {
            return new Promise(resolve => {
              const list = [
                { id: 10001, name: 'Test1', role: 'Develop', sex: 'Man', age: 28, address: 'Shenzhen' },
                { id: 10002, name: 'Test2', role: 'Test', sex: 'Women', age: 22, address: 'Guangzhou' },
                { id: 10003, name: 'Test3', role: 'PM', sex: 'Man', age: 32, address: 'Shanghai' },
                { id: 10004, name: 'Test4', role: 'Designer', sex: 'Women', age: 23, address: 'Shenzhen' },
                { id: 10005, name: 'Test5', role: 'Develop', sex: 'Women', age: 30, address: 'Shanghai' },
                { id: 10006, name: 'Test6', role: 'Designer', sex: 'Women', age: 21, address: 'Shenzhen' },
                { id: 10007, name: 'Test7', role: 'Test', sex: 'Man', age: 29, address: 'Shenzhen' },
                { id: 10008, name: 'Test8', role: 'Develop', sex: 'Man', age: 35, address: 'Shenzhen' }
              ]
              resolve(list)
            })
          }
        }
      },
      demoCodes: [
        `
        // 创建一个简单的表单-输入框渲染
        VXETable.renderer.add('FormItemInput', {
          // 项内容模板
          renderItemContent (h, renderOpts, params) {
            const { data, property } = params
            const props = renderOpts.props || {}
            return [
              <vxe-input v-model={ data[property] } { ...{ props } }></vxe-input>
            ]
          }
        })
        `,
        `
        <vxe-grid
          border
          resizable
          height="400"
          :export-config="{}"
          :toolbar-config="{export: true, custom: true}"
          :form-config="tableForm"
          :proxy-config="tableProxy"
          :columns="tableColumn">
        </vxe-grid>
        `,
        `
        export default {
          data () {
            return {
              tableColumn: [
                { type: 'seq', width: 50 },
                { field: 'name', title: 'Name' },
                { field: 'age', title: 'Age' },
                { field: 'sex', title: 'Sex' },
                { field: 'role', title: 'Role' }
              ],
              tableForm: {
                items: [
                  { field: 'name', title: '名称', itemRender: { name: 'FormItemInput', props: { clearable: true, placeholder: '请输入名称' } } },
                  { field: 'age', title: '年龄', itemRender: { name: 'FormItemInput', props: { type: 'number', clearable: true, placeholder: '请输入年龄' } } },
                  { itemRender: { name: 'input', attrs: { type: 'submit', value: '查询' } } },
                  { itemRender: { name: 'input', attrs: { type: 'reset', value: '重置' } } }
                ]
              },
              tableProxy: {
                form: true,
                ajax: {
                  query: () => {
                    return new Promise(resolve => {
                      const list = [
                        { id: 10001, name: 'Test1', role: 'Develop', sex: 'Man', age: 28, address: 'Shenzhen' },
                        { id: 10002, name: 'Test2', role: 'Test', sex: 'Women', age: 22, address: 'Guangzhou' },
                        { id: 10003, name: 'Test3', role: 'PM', sex: 'Man', age: 32, address: 'Shanghai' },
                        { id: 10004, name: 'Test4', role: 'Designer', sex: 'Women', age: 23, address: 'Shenzhen' },
                        { id: 10005, name: 'Test5', role: 'Develop', sex: 'Women', age: 30, address: 'Shanghai' },
                        { id: 10006, name: 'Test6', role: 'Designer', sex: 'Women', age: 21, address: 'Shenzhen' },
                        { id: 10007, name: 'Test7', role: 'Test', sex: 'Man', age: 29, address: 'Shenzhen' },
                        { id: 10008, name: 'Test8', role: 'Develop', sex: 'Man', age: 35, address: 'Shenzhen' }
                      ]
                      resolve(list)
                    })
                  }
                }
              }
            }
          }
        }
        `
      ]
    }
  }
}
</script>
