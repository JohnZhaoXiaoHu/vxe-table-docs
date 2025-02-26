<template>
  <div>
    <p class="tip">
      使用自定义模板渲染，通过 <table-column-api-link prop="slots"/> 属性使用自定义插槽来编写模板或 <a class="link" href="https://cn.vuejs.org/v2/guide/render-function.html#JSX" target="_blank">JSX</a> 渲染函数或 <a class="link" href="https://cn.vuejs.org/v2/guide/render-function.html#%E8%99%9A%E6%8B%9F-DOM" target="_blank">VNode</a><span class="red">（注：返回数组格式）</span><br>
      <grid-api-link prop="form"/>：自定义表单模板<br>
      <grid-api-link prop="toolbar"/>：自定义工具栏模板<br>
      <grid-api-link prop="top"/>：自定义顶部模板<br>
      <grid-api-link prop="bottom"/>：自定义底部模板<br>
      <grid-api-link prop="pager"/>：自定义分页模板<br>
    </p>

    <vxe-grid
      border
      resizable
      show-footer
      height="600"
      ref="xGrid"
      class="my-grid66"
      :footer-method="footerMethod"
      :columns="tableColumn"
      :data="tableData"
      :toolbar-config="tableToolbar"
      :edit-config="{trigger: 'click', mode: 'cell', icon: 'fa fa-pencil-square-o'}"
      @checkbox-change="checkboxChangeEvent"
      @checkbox-all="checkboxChangeEvent">
      <!--使用 form 插槽-->
      <template v-slot:form>
        <vxe-form :data="formData" @submit="searchEvent">
          <vxe-form-item title="名称" field="name">
            <template v-slot>
              <vxe-input v-model="formData.name" placeholder="请输入名称" clearable></vxe-input>
            </template>
          </vxe-form-item>
          <vxe-form-item title="昵称" field="nickname">
            <template v-slot>
              <vxe-input v-model="formData.nickname" placeholder="请输入昵称" clearable></vxe-input>
            </template>
          </vxe-form-item>
          <vxe-form-item title="性别" field="sex">
            <template v-slot>
              <vxe-select v-model="formData.sex" placeholder="请选择性别" clearable>
                <vxe-option value="1" label="女"></vxe-option>
                <vxe-option value="2" label="男"></vxe-option>
              </vxe-select>
            </template>
          </vxe-form-item>
          <vxe-form-item>
            <template v-slot>
              <vxe-button status="primary">查询</vxe-button>
            </template>
          </vxe-form-item>
        </vxe-form>
      </template>

      <!--自定义插槽 toolbar buttons 插槽-->
      <template v-slot:toolbar_buttons>
        <vxe-form>
          <vxe-form-item>
            <template v-slot>
              <vxe-input placeholder="搜索"></vxe-input>
            </template>
          </vxe-form-item>
          <vxe-form-item>
            <template v-slot>
              <vxe-button status="primary">查询</vxe-button>
            </template>
          </vxe-form-item>
        </vxe-form>
      </template>

      <!--自定义插槽 toolbar tools 插槽-->
      <template v-slot:toolbar_tools>
        <vxe-input placeholder="搜索"></vxe-input>
      </template>

      <!--使用 top 插槽-->
      <template v-slot:top>
        <div class="alert-message">
          <i class="fa fa-exclamation-circle alert-message-icon"></i>
          <span class="alert-message-content">
            <marquee direction="left" scrollamount="4" width="100%" onmouseover="this.stop();" onmouseout="this.start();">自定义模板</marquee>
          </span>
        </div>
      </template>

      <!--自定义插槽 seq_header-->
      <template v-slot:seq_header>
        <div class="first-col">
          <div class="first-col-top">名称</div>
          <div class="first-col-bottom">序号</div>
        </div>
      </template>

      <!--自定义插槽 Number-->
      <template v-slot:num_default="{ row, rowIndex }">
        <template v-if="rowIndex === 2">
          <vxe-switch v-model="row.flag"></vxe-switch>
        </template>
        <template v-else-if="rowIndex === 3">
          <vxe-switch v-model="row.flag" open-label="开" close-label="关"></vxe-switch>
        </template>
        <template v-else>
          <span>{{ row.num1 }}</span>
        </template>
      </template>
      <template v-slot:num_footer="{ items, _columnIndex }">
        <span style="color: red">合计：{{ items[_columnIndex] }}</span>
      </template>

      <!--自定义插槽 name_default-->
      <template v-slot:name_default="{ row, column }">
        <span>
          <span style="color: red;">{{ row.name }}</span>
          <button @click="showDetailEvent(row, column)">弹框</button>
        </span>
      </template>

      <!--自定义插槽 html1-->
      <template v-slot:html1_default="{ row, rowIndex }">
        <template v-if="rowIndex === 1">
          <vxe-select v-model="row.flag1" transfer>
            <vxe-option value="Y" label="是"></vxe-option>
            <vxe-option value="N" label="否"></vxe-option>
          </vxe-select>
        </template>
        <template v-else>
          <span v-html="row.html1"></span>
        </template>
      </template>

      <!--使用 bottom 插槽-->
      <template v-slot:bottom>
        <div class="alert-message">
          <i class="fa fa-exclamation-circle alert-message-icon"></i>
          <span class="alert-message-content">
            <marquee direction="right" scrollamount="4" width="100%" onmouseover="this.stop();" onmouseout="this.start();">自定义模板</marquee>
          </span>
        </div>
      </template>

      <!--自定义插槽 pager 插槽-->
      <template v-slot:pager>
        <vxe-pager
          perfect
          :current-page="tablePage.currentPage"
          :page-size="tablePage.pageSize"
          :total="tablePage.totalResult">
          <template v-slot:left>
            <span class="page-left">
              <vxe-checkbox v-model="isAllChecked" :indeterminate="isIndeterminate" @change="changeAllEvent"></vxe-checkbox>
              <span class="select-count">已选中 {{ selectRecords.length }} 条</span>
              <vxe-button>修改</vxe-button>
              <vxe-button>管理</vxe-button>
              <vxe-button>删除</vxe-button>
              <vxe-button size="small">
                <template v-slot>更多操作</template>
                <template v-slot:dropdowns>
                  <vxe-button type="text">批量修改</vxe-button>
                  <vxe-button type="text">批量管理</vxe-button>
                  <vxe-button type="text">批量删除</vxe-button>
                </template>
              </vxe-button>
            </span>
          </template>
          <template v-slot:right>
            <span>
              <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
              <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
              <img src="/vxe-table/static/other/img2.gif" style="height: 30px;" />
              <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
              <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
            </span>
          </template>
        </vxe-pager>
      </template>
    </vxe-grid>

    <vxe-modal v-model="showDetails" title="查看详情" width="800" height="400" resize>
      <template v-slot>
        <div v-if="selectRow" v-html="selectRow.html3"></div>
      </template>
    </vxe-modal>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[0] }}</code>
      <code class="javascript">{{ demoCodes[1] }}</code>
      <code class="css">{{ demoCodes[2] }}</code>
    </pre>
  </div>
</template>

<script>
import XEUtils from 'xe-utils'
import hljs from 'highlight.js'

export default {
  data () {
    return {
      showDetails: false,
      selectRow: null,
      isAllChecked: false,
      isIndeterminate: false,
      selectRecords: [],
      formData: {
        name: '',
        nickname: '',
        sex: ''
      },
      tablePage: {
        total: 0,
        currentPage: 1,
        pageSize: 10
      },
      tableData: [],
      tableToolbar: {
        custom: true,
        slots: {
          buttons: 'toolbar_buttons',
          tools: 'toolbar_tools'
        }
      },
      tableColumn: [
        { type: 'checkbox', width: 60 },
        // 对应自定义插槽的名称
        { type: 'seq', width: 100, resizable: false, slots: { header: 'seq_header' } },
        {
          field: 'name',
          title: 'Name',
          slots: {
            default: 'name_default',
            footer: () => {
              return [
                <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />,
                <span>浪里个浪</span>,
                <img src="/vxe-table/static/other/img2.gif" style="height: 30px;" />
              ]
            }
          }
        },
        {
          field: 'num1',
          title: 'Money',
          showHeaderOverflow: true,
          editRender: { autofocus: '.my-input' },
          slots: {
            // 使用 JSX 渲染
            default: ({ row }) => {
              return [
                <span>￥{ row.num1 }元</span>
              ]
            },
            header: ({ column }) => {
              return [
                <span>
                  <i>@</i>
                  <span style="color: red;" onClick={ this.headerClickEvent }>{ column.title }</span>
                </span>
              ]
            },
            footer: ({ items, _columnIndex }) => {
              return [
                <vxe-button status="primary" size="mini">按钮</vxe-button>,
                <span>累计：{ items[_columnIndex] }</span>
              ]
            },
            filter: ({ column, $panel }) => {
              return column.filters.map(option => {
                return <input type="type" v-model={ option.data } onInput={ evnt => this.changeFilterEvent(evnt, option, $panel) }/>
              })
            },
            edit: ({ row }) => {
              return [
                <input type="number" class="my-input" v-model={ row.num1 } />
              ]
            }
          }
        },
        {
          field: 'num',
          title: 'Number',
          slots: {
            // 对应自定义插槽的名称
            default: 'num_default',
            footer: 'num_footer'
          }
        },
        {
          field: 'address',
          title: 'Address',
          showOverflow: true,
          slots: {
            // 使用渲染函数
            default: ({ row }) => {
              return [
                <span style="color: blue" onClick={ () => this.addressClickEvent(row) }>{ row.address }</span>
              ]
            }
          }
        },
        {
          field: 'html1',
          title: 'Html片段',
          showOverflow: true,
          slots: {
            // 对应自定义插槽的名称
            default: 'html1_default'
          }
        },
        {
          field: 'img1',
          title: '图片路径',
          slots: {
            // 使用 JSX 渲染
            default: ({ row }) => {
              return [
                row.img1 ? <img src={ row.img1 } style="width: 100px;"/> : <span>无</span>
              ]
            }
          }
        }
      ],
      demoCodes: [
        `
        <vxe-grid
          border
          resizable
          show-footer
          height="600"
          ref="xGrid"
          class="my-grid66"
          :footer-method="footerMethod"
          :columns="tableColumn"
          :data="tableData"
          :toolbar-config="tableToolbar"
          :edit-config="{trigger: 'click', mode: 'cell', icon: 'fa fa-pencil-square-o'}"
          @checkbox-change="checkboxChangeEvent"
          @checkbox-all="checkboxChangeEvent">
          <!--使用 form 插槽-->
          <template v-slot:form>
            <vxe-form :data="formData" @submit="searchEvent">
              <vxe-form-item title="名称" field="name">
                <template v-slot>
                  <vxe-input v-model="formData.name" placeholder="请输入名称" clearable></vxe-input>
                </template>
              </vxe-form-item>
              <vxe-form-item title="昵称" field="nickname">
                <template v-slot>
                  <vxe-input v-model="formData.nickname" placeholder="请输入昵称" clearable></vxe-input>
                </template>
              </vxe-form-item>
              <vxe-form-item title="性别" field="sex">
                <template v-slot>
                  <vxe-select v-model="formData.sex" placeholder="请选择性别" clearable>
                    <vxe-option value="1" label="女"></vxe-option>
                    <vxe-option value="2" label="男"></vxe-option>
                  </vxe-select>
                </template>
              </vxe-form-item>
              <vxe-form-item>
                <template v-slot>
                  <vxe-button status="primary">查询</vxe-button>
                </template>
              </vxe-form-item>
            </vxe-form>
          </template>

          <!--自定义插槽 toolbar buttons 插槽-->
          <template v-slot:toolbar_buttons>
            <vxe-form>
              <vxe-form-item>
                <template v-slot>
                  <vxe-input placeholder="搜索"></vxe-input>
                </template>
              </vxe-form-item>
              <vxe-form-item>
                <template v-slot>
                  <vxe-button status="primary">查询</vxe-button>
                </template>
              </vxe-form-item>
            </vxe-form>
          </template>

          <!--自定义插槽 toolbar tools 插槽-->
          <template v-slot:toolbar_tools>
            <vxe-input placeholder="搜索"></vxe-input>
          </template>

          <!--使用 top 插槽-->
          <template v-slot:top>
            <div class="alert-message">
              <i class="fa fa-exclamation-circle alert-message-icon"></i>
              <span class="alert-message-content">
                <marquee direction="left" scrollamount="4" width="100%" onmouseover="this.stop();" onmouseout="this.start();">自定义模板</marquee>
              </span>
            </div>
          </template>

          <!--自定义插槽 seq_header-->
          <template v-slot:seq_header>
            <div class="first-col">
              <div class="first-col-top">名称</div>
              <div class="first-col-bottom">序号</div>
            </div>
          </template>

          <!--自定义插槽 Number-->
          <template v-slot:num_default="{ row, rowIndex }">
            <template v-if="rowIndex === 2">
              <vxe-switch v-model="row.flag"></vxe-switch>
            </template>
            <template v-else-if="rowIndex === 3">
              <vxe-switch v-model="row.flag" open-label="开" close-label="关"></vxe-switch>
            </template>
            <template v-else>
              <span>{{ row.num1 }}</span>
            </template>
          </template>
          <template v-slot:num_footer="{ items, _columnIndex }">
            <span style="color: red">合计：{{ items[_columnIndex] }}</span>
          </template>

          <!--自定义插槽 name_default-->
          <template v-slot:name_default="{ row, column }">
            <span>
              <span style="color: red;">{{ row.name }}</span>
              <button @click="showDetailEvent(row, column)">弹框</button>
            </span>
          </template>

          <!--自定义插槽 html1-->
          <template v-slot:html1_default="{ row, rowIndex }">
            <template v-if="rowIndex === 1">
              <vxe-select v-model="row.flag1" transfer>
                <vxe-option value="Y" label="是"></vxe-option>
                <vxe-option value="N" label="否"></vxe-option>
              </vxe-select>
            </template>
            <template v-else>
              <span v-html="row.html1"></span>
            </template>
          </template>

          <!--使用 bottom 插槽-->
          <template v-slot:bottom>
            <div class="alert-message">
              <i class="fa fa-exclamation-circle alert-message-icon"></i>
              <span class="alert-message-content">
                <marquee direction="right" scrollamount="4" width="100%" onmouseover="this.stop();" onmouseout="this.start();">自定义模板</marquee>
              </span>
            </div>
          </template>

          <!--自定义插槽 pager 插槽-->
          <template v-slot:pager>
            <vxe-pager
              perfect
              :current-page="tablePage.currentPage"
              :page-size="tablePage.pageSize"
              :total="tablePage.totalResult">
              <template v-slot:left>
                <span class="page-left">
                  <vxe-checkbox v-model="isAllChecked" :indeterminate="isIndeterminate" @change="changeAllEvent"></vxe-checkbox>
                  <span class="select-count">已选中 {{ selectRecords.length }} 条</span>
                  <vxe-button>修改</vxe-button>
                  <vxe-button>管理</vxe-button>
                  <vxe-button>删除</vxe-button>
                  <vxe-button size="small">
                    <template v-slot>更多操作</template>
                    <template v-slot:dropdowns>
                      <vxe-button type="text">批量修改</vxe-button>
                      <vxe-button type="text">批量管理</vxe-button>
                      <vxe-button type="text">批量删除</vxe-button>
                    </template>
                  </vxe-button>
                </span>
              </template>
              <template v-slot:right>
                <span>
                  <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
                  <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
                  <img src="/vxe-table/static/other/img2.gif" style="height: 30px;" />
                  <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
                  <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />
                </span>
              </template>
            </vxe-pager>
          </template>
        </vxe-grid>

        <vxe-modal v-model="showDetails" title="查看详情" width="800" height="400" resize>
          <template v-slot>
            <div v-if="selectRow" v-html="selectRow.html3"></div>
          </template>
        </vxe-modal>
        `,
        `
        export default {
          data () {
            return {
              showDetails: false,
              selectRow: null,
              isAllChecked: false,
              isIndeterminate: false,
              selectRecords: [],
              formData: {
                name: '',
                nickname: '',
                sex: ''
              },
              tablePage: {
                total: 0,
                currentPage: 1,
                pageSize: 10
              },
              tableData: [],
              tableToolbar: {
                custom: true,
                slots: {
                  buttons: 'toolbar_buttons',
                  tools: 'toolbar_tools'
                }
              },
              tableColumn: [
                { type: 'checkbox', width: 60 },
                // 对应自定义插槽的名称
                { type: 'seq', width: 100, resizable: false, slots: { header: 'seq_header' } },
                {
                  field: 'name',
                  title: 'Name',
                  slots: {
                    default: 'name_default',
                    footer: () => {
                      return [
                        <img src="/vxe-table/static/other/img1.gif" style="height: 30px;" />,
                        <span>浪里个浪</span>,
                        <img src="/vxe-table/static/other/img2.gif" style="height: 30px;" />
                      ]
                    }
                  }
                },
                {
                  field: 'num1',
                  title: 'Money',
                  showHeaderOverflow: true,
                  editRender: { autofocus: '.my-input' },
                  slots: {
                    // 使用 JSX 渲染
                    default: ({ row }) => {
                      return [
                        <span>￥{ row.num1 }元</span>
                      ]
                    },
                    header: ({ column }) => {
                      return [
                        <span>
                          <i>@</i>
                          <span style="color: red;" onClick={ this.headerClickEvent }>{ column.title }</span>
                        </span>
                      ]
                    },
                    footer: ({ items, _columnIndex }) => {
                      return [
                        <vxe-button status="primary" size="mini">按钮</vxe-button>,
                        <span>累计：{ items[_columnIndex] }</span>
                      ]
                    },
                    filter: ({ column, $panel }) => {
                      return column.filters.map(option => {
                        return <input type="type" v-model={ option.data } onInput={ evnt => this.changeFilterEvent(evnt, option, $panel) }/>
                      })
                    },
                    edit: ({ row }) => {
                      return [
                        <input type="number" class="my-input" v-model={ row.num1 } />
                      ]
                    }
                  }
                },
                {
                  field: 'num',
                  title: 'Number',
                  slots: {
                    // 对应自定义插槽的名称
                    default: 'num_default',
                    footer: 'num_footer'
                  }
                },
                {
                  field: 'address',
                  title: 'Address',
                  showOverflow: true,
                  slots: {
                    // 使用渲染函数
                    default: ({ row }) => {
                      return [
                        <span style="color: blue" onClick={ () => this.addressClickEvent(row) }>{ row.address }</span>
                      ]
                    }
                  }
                },
                {
                  field: 'html1',
                  title: 'Html片段',
                  showOverflow: true,
                  slots: {
                    // 对应自定义插槽的名称
                    default: 'html1_default'
                  }
                },
                {
                  field: 'img1',
                  title: '图片路径',
                  slots: {
                    // 使用 JSX 渲染
                    default: ({ row }) => {
                      return [
                        row.img1 ? <img src={ row.img1 } style="width: 100px;"/> : <span>无</span>
                      ]
                    }
                  }
                }
              ]
            }
          },
          created () {
            this.tableData = window.MOCK_DATA_LIST.slice(0, 8)
          },
          methods: {
            searchEvent () {
              this.$XModal.alert('查询')
            },
            showDetailEvent (row) {
              this.selectRow = row
              this.showDetails = true
            },
            headerClickEvent (evnt) {
              this.$XModal.alert('头部点击事件')
            },
            addressClickEvent (row) {
              this.$XModal.alert(\`address点击事件：\${row.address}\`)
            },
            changeFilterEvent (evnt, option, $panel) {
              $panel.changeOption(evnt, !!option.data, option)
            },
            checkboxChangeEvent ({ records }) {
              this.isAllChecked = this.$refs.xGrid.isAllCheckboxChecked()
              this.isIndeterminate = this.$refs.xGrid.isAllCheckboxIndeterminate()
              this.selectRecords = records
            },
            changeAllEvent () {
              this.$refs.xGrid.setAllCheckboxRow(this.isAllChecked)
              this.selectRecords = this.$refs.xGrid.getCheckboxRecords()
            },
            footerMethod ({ columns, data }) {
              return [
                columns.map((column, columnIndex) => {
                  if (['num1', 'num'].includes(column.property)) {
                    return XEUtils.sum(data, column.property)
                  }
                  return null
                })
              ]
            }
          }
        }
        `,
        `
        .my-grid66 .alert-message {
          height: 40px;
          display: flex;
          align-items: center;
          margin: 10px 0;
          border-radius: 4px;
          background-color: #e6f7ff;
          border: 1px solid #91d5ff;
        }
        .my-grid66 .alert-message-icon {
          width: 30px;
          text-align: center;
          color: #409eff;
          margin-right: 8px;
        }
        .my-grid66 .alert-message-content {
          flex-grow: 1;
          padding-right: 20px;
        }
        .my-grid66 .page-left {
          position: absolute;
          left: 10px;
          top: 50%;
          transform: translateY(-50%);
          z-index: 10;
        }
        .my-grid66 .select-count {
          display: inline-block;
          vertical-align: middle;
        }
        .my-grid66 .my-input {
          width: 100%;
        }
        .my-grid66 .first-col {
          position: relative;
          height: 20px;
        }
        .my-grid66 .first-col:before {
          content: "";
          position: absolute;
          left: -15px;
          top: 10px;
          width: 110px;
          height: 1px;
          transform: rotate(28deg);
          background-color: #e8eaec;
        }
        .my-grid66 .first-col .first-col-top {
          position: absolute;
          right: 4px;
          top: -10px;
        }
        .my-grid66 .first-col .first-col-bottom {
          position: absolute;
          left: 4px;
          bottom: -10px;
        }
        `
      ]
    }
  },
  created () {
    this.tableData = window.MOCK_DATA_LIST.slice(0, 8)
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    searchEvent () {
      this.$XModal.alert('查询')
    },
    showDetailEvent (row) {
      this.selectRow = row
      this.showDetails = true
    },
    headerClickEvent () {
      this.$XModal.alert('头部点击事件')
    },
    addressClickEvent (row) {
      this.$XModal.alert(`address点击事件：${row.address}`)
    },
    changeFilterEvent (evnt, option, $panel) {
      $panel.changeOption(evnt, !!option.data, option)
    },
    checkboxChangeEvent ({ records }) {
      this.isAllChecked = this.$refs.xGrid.isAllCheckboxChecked()
      this.isIndeterminate = this.$refs.xGrid.isAllCheckboxIndeterminate()
      this.selectRecords = records
    },
    changeAllEvent () {
      this.$refs.xGrid.setAllCheckboxRow(this.isAllChecked)
      this.selectRecords = this.$refs.xGrid.getCheckboxRecords()
    },
    footerMethod ({ columns, data }) {
      return [
        columns.map((column) => {
          if (['num1', 'num'].includes(column.property)) {
            return XEUtils.sum(data, column.property)
          }
          return null
        })
      ]
    }
  }
}
</script>

<style scoped>
.my-grid66 .alert-message {
  height: 40px;
  display: flex;
  align-items: center;
  margin: 10px 0;
  border-radius: 4px;
  background-color: #e6f7ff;
  border: 1px solid #91d5ff;
}
.my-grid66 .alert-message-icon {
  width: 30px;
  text-align: center;
  color: #409eff;
  margin-right: 8px;
}
.my-grid66 .alert-message-content {
  flex-grow: 1;
  padding-right: 20px;
}
.my-grid66 .page-left {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  z-index: 10;
}
.my-grid66 .select-count {
  display: inline-block;
  vertical-align: middle;
}
.my-grid66 .my-input {
  width: 100%;
}
.my-grid66 .first-col {
  position: relative;
  height: 20px;
}
.my-grid66 .first-col:before {
  content: "";
  position: absolute;
  left: -15px;
  top: 10px;
  width: 110px;
  height: 1px;
  transform: rotate(28deg);
  background-color: #e8eaec;
}
.my-grid66 .first-col .first-col-top {
  position: absolute;
  right: 4px;
  top: -10px;
}
.my-grid66 .first-col .first-col-bottom {
  position: absolute;
  left: 4px;
  bottom: -10px;
}
</style>
