<template>
  <div class="CCheckbox" @click="ControlClick()">
    <div v-if="config && (!ControlID)" @click.stop>
      <div class="title">
        {{config.CTitleCN}}
      </div>
       <!--初始化组件-->
    </div>
    <div v-else>
      <div class="title">
        {{ControlConfig.CTitleCN}}
      </div>
      <div v-if="ControlConfig.CAttribute.typeModel==='checkbox'">
        <div>
          <el-checkbox
            :indeterminate="ControlConfig.CAttribute.indeterminateCheckbox" v-model="ControlConfig.CAttribute.checkAllCheckbox"
            @change="CheckAllChange"
            v-if="ControlConfig.CAttribute.showAllCheckbox">全选</el-checkbox>
        </div>
        <el-checkbox-group
          v-model="ControlConfig.CAttribute.defaultCheckboxSelected"
          @change="SelectedChange"
          :min="ControlConfig.CAttribute.ableSelectedMin"
          :max="ControlConfig.CAttribute.ableSelectedMax">
          <span class="checkbox"
                @click="SelectedChange(item.label)"
                v-for="(item, index) in ControlConfig.CAttribute.itemAttr"
                :key="item.label">
              <el-checkbox
                :label="item.label"
                :disabled="item.isDisabled">
                {{item.showContent}}</el-checkbox>
          </span>
        </el-checkbox-group>
      </div>
      <div v-else>
        <el-checkbox-group
          v-model="ControlConfig.CAttribute.defaultCheckboxSelected"
          :size="ControlConfig.CAttribute.sizeModel"
          :text-color="ControlConfig.CAttribute.textColor"
          :fill="ControlConfig.CAttribute.fillColor"
          :min="ControlConfig.CAttribute.ableSelectedMin"
          :max="ControlConfig.CAttribute.ableSelectedMax">
          <span class="checkbox"
                @click="SelectedChange(item.label)"
                v-for="(item, index) in ControlConfig.CAttribute.itemAttr"
                :key="item.label">
              <el-checkbox-button
                :label="item.label"
                :disabled="item.isDisabled">
                {{item.showContent}}</el-checkbox-button>
          </span>
        </el-checkbox-group>
      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    name: `CCheckbox`,
    created () {
      this.config = this.initConfig
      if (this.ControlConfig) {
        this.config = this.ControlConfig
      }
      if (this.ControlID && (!this.config.ControlID)) {
        this.config.ControlID = this.ControlID
      }
    },
    mounted () {
      if (this.ControlConfig) {
        this.config = this.ControlConfig
      }
      if (this.ControlID && (!this.config.ControlID)) {
        this.config.ControlID = this.ControlID
      }
      this.$emit('input', this.config)
    },
    props: {
      ControlConfig: {
        type: Object
      },
      ControlID: {
        type: String,
        default: null
      }
    },
    methods: {
      emitConfig () {
        this.config = this.initConfig
        if (this.ControlConfig) {
          this.config = this.ControlConfig
        }
        if (this.ControlID && (!this.config.ControlID)) {
          this.config.ControlID = this.ControlID
        }
        this.$emit(`getValue`, this.config)
      },
      ControlClick () {
        this.emitConfig()
      },
      SelectedChange (label) {
        this.config.CAttribute.itemAttr.forEach((item, index) => {
          if (label[label.length - 1] === item.label) {
            this.config.CAttribute.currentSelected = index
          }
        })
      },
      CheckAllChange (event) {
        if (event.target.checked) {
          this.config.CAttribute.defaultCheckboxSelected = this.config.CAttribute.itemAttr.map((item) => {
            return item.label
          })
        }
        this.config.CAttribute.indeterminateCheckbox = false
      }
    },
    data () {
      return {
        initConfig: {
          ControlID: '', // 表单生成后的控件id
          CBelong: 'form',
          CTitleCN: '多选框', // 标题
          CTitleEN: 'checkbox Control', // 英文标题
          CName: 'CCheckbox', // 控件名称
          CLayout: { // 布局
            percentLayout: { // 百分比布局
              type: Number,
              default: 100,
              status: true
            },
            pixelLayout: { // 像素布局
              type: Number,
              default: 100,
              status: true
            },
            flexLayout: { // flex 布局
              type: Number,
              default: 1,
              status: false
            },
            columnLayout: { // 栅格布局
              type: Number,
              default: 12,
              status: false
            }
          },
          CAttribute: {
            type: [{
              value: 'checkbox',
              name: '普通样式'
            }, {
              value: 'button',
              name: '按钮样式'
            }], // Checkbox 类型 普通类型和按钮类型
            typeModel: 'checkbox',
            description: '', // 描述
            size: [{
              value: 'large',
              name: '大'
            }, {
              value: 'small',
              name: '小'
            }], // 按钮尺寸
            sizeModel: 'small',
            itemAttr: [{
              label: '1',
              showContent: '样例1',
              isDisabled: false // 是否禁用该选项
            }, {
              label: '2',
              showContent: '样例2',
              isDisabled: false // 是否禁用该选项
            }],
            indeterminateCheckbox: true, // 全选样式
            checkAllCheckbox: true, // 默认全选选中
            showAllCheckbox: false, // 是否显示全选选项
            addStatus: false, // 增加Checkbox选项状态
            currentSelected: 0, // 现在选中项的index
            defaultCheckboxSelected: [], // 默认选中项
            textColor: '#fff', // 按钮激活时的文本颜色
            fillColor: '', // 按钮激活时的填充色和边框色（默认继承主题样式：设置default）
            ableSelectedMin: 1, // 可被勾选的 checkbox 的最小数量
            ableSelectedMax: 2 // 可被勾选的 checkbox 的最大数量
          },
          CKey: { // 控件值
            default: '', // 默认值
            type: '', // 控件值类型
            keyMethods: '' // 计算控件值方法
          },
          Status: { // 状态
            status: false, // 是否应用状态
            rules: [
              {
                value: 'readonly',
                name: '只读'
              },
              {
                value: '',
                name: '隐藏'
              },
              {
                value: '',
                name: '禁用'
              }
            ], // 控件规则集合
            ruleList: [] // 选择集合
          },
          CValidate: {
            status: false,
            chooseStatus: false,
            validateModel: ''
          },
          methodDB: [{
            name: '提交', // 中文名称（Example）
            methodName: 'save', // 英文名称 (Example)
            action: '/form/saveAction' // postAction(接口名称)
          }]
        },
        currentConfig: null,
        config: null,
        validate: ''
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "~assets/css/stylus/mixin"
  .title
    padding 8px 0px
    color $font-primary
    font-size $font-medium
</style>
