<!--
描述：办公自动化-红头文件申请-修改组件
开发人：sunlf
开发日期：2017年12月20日
-->
<template lang="pug">
  div.el-form.kalix-form-table(slot="dialogFormSlot")
    div.table-title 吉林动画学院红头文件申请表
    el-form-item(label="名称" v-bind:label-width="labelWidth" prop="title" v-bind:rules="rules.title")
      el-input(v-model="formModel.title")
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="申请部门" v-bind:label-width="labelWidth" prop="orgId" v-bind:rules="rules.orgId")
        kalix-org-select(v-model="formModel.orgId" v-on:selectChange="onOrgIdChange")
      <!--el-form-item.s-flex_item(label="文号类型" v-bind:label-width="labelWidth"  prop="docAssort" v-bind:rules="rules.docAssort")-->
        <!--el-cascader(expand-trigger="hover"    v-bind:options="options"    v-model="formModel.docAssort" v-bind:show-all-levels="false"  v-on:change="onChange")-->
      el-form-item.s-flex_item(label="文号类型" prop="docType" v-bind:rules="rules.docType" v-bind:label-width="labelWidth")
        template(v-if="formModel.editDocType")
          kalix-dict-select(v-model="formModel.docType" appName="oa" dictType="文号类型")
        template(v-else)
          <!--kalix-dict-select(v-model="formModel.docType" appName="oa" dictType="文号类型" disabled)-->
          el-input(v-bind:value="formModel.docType|getDictName('OA','文号类型')" readonly)
    div
      el-form-item.kalix-form-table-td(label="发文内容" v-bind:label-width="labelWidth" prop="docContent" v-bind:rules="rules.docContent")
        <!--el-input(v-model="formModel.docContent" type="textarea")-->
        tinymce(v-model="formModel.docContent" ref="editor")
    div
      el-form-item.kalix-form-table-td(label="审批选项" v-bind:label-width="labelWidth")
        div(style="text-align:center")
          el-switch(v-model="formModel.needHeader" active-text="需要校领导审批" inactive-text="不需要校领导审批" v-on:change="switchChange")
    template(v-if="formModel.needHeader")
      div
        el-form-item.kalix-form-table-td(label="校领导审批名单" prop="needManagerUser" v-bind:rules="rules.needManagerUser" v-bind:label-width="labelWidth")
          kalix-checkbox(v-model="formModel.needManagerUser" v-bind:dataUrl="managerUsersURL")
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="部门负责人签字" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.depUser" readonly)
      el-form-item.s-flex_item.kalix-form-table-td(label="校务部签字" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.schoolUser" readonly)
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="校领导签字" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.managerUser" readonly)
      el-form-item.s-flex_item.kalix-form-table-td(label="董事长签字" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.chairmanUser" readonly)
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="经办人" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.createBy" readonly)
</template>

<script type="text/ecmascript-6">
  import {RedheadApplyURL} from '../config.toml'
  import {ManagerUserDutyID} from '@/config/global.toml'
  import UserOrgSelect from '@/components/biz/select/UserOrgSelect'
  import Dialog from '@/components/custom/baseDialog.vue'
  import BaseDictSelect from '@/components/custom/baseDictSelect'
  import Tinymce from '@/third/Tinymce'
  import BaseCheckbox from '@/components/custom/baseCheckbox'

  export default {
    props: {
      formModel: {
        type: Object,
        default: {}
      }
    },
    created() {
    },
    data() {
      return {
        targetURL: RedheadApplyURL,
        rules: {
          title: [{required: true, message: '请输入名称', trigger: 'blur'}],
          orgId: [{type: 'number', required: true, message: '请选择申请部门', trigger: 'change'}],
//          docAssort: [{required: true, message: '请选择文号类型', trigger: 'change'}],
          docType: [{type: 'number', required: true, message: '请选择文号类型', trigger: 'change'}],
          docContent: [{required: true, message: '请输入发文内容', trigger: 'blur'}],
          needManagerUser: [{required: true, message: '请选择需要审批的校领导', trigger: 'blur'}]
        },
        managerUsersURL: '/camel/rest/dutys/' + ManagerUserDutyID + '/users/loginnames',
//        options: [{
//          value: '对外',
//          label: '对外',
//          children: [{
//            label: '吉动院字',
//            value: '1'
//          }]
//        }, {
//          value: '对内',
//          label: '对内',
//          children: [
//            {
//              label: '吉动院人字',
//              value: '2'
//            },
//            {
//              label: '吉动院教字',
//              value: '3'
//            },
//            {
//              label: '吉动院学字',
//              value: '4'
//            },
//            {
//              label: '吉动院招字',
//              value: '5'
//            },
//            {
//              label: '吉动院研字',
//              value: '6'
//            },
//            {
//              label: '吉动院学位字',
//              value: '7'
//            },
//            {
//              label: '吉动校友字',
//              value: '8'
//            },
//            {
//              label: '吉动院研学字',
//              value: '9'
//            },
//            {
//              label: '吉动院字',
//              value: '10'
//            }
//          ]
//        }],
        labelWidth: '140px'
      }
    },
    methods: {
      kalixDialogOpen() {
        this.$nextTick(() => {
          console.log('%c ==== KalixDialogOpen ==== ', 'color:#FFF;background:#055', this.$refs.editor)
          this.$refs.editor.initTinymce()
        })
      },
      kalixDialogClose() {
        this.$refs.editor.destroyTinymce()
      },
      onOrgIdChange(item) {
        this.formModel.orgName = item.name
      },
//      onChange(value) {
//        this.formModel.docType = value[1]
//      },
//      init(val) {
//        if (val > 1) {
//          this.formModel.docAssort = ['对内', String(val)]
//        } else {
//          this.formModel.docAssort = ['对外', '1']
//        }
//      },
      switchChange(newStatus) {
        if (!newStatus) {
          this.formModel.needManagerUser = ''
        }
      }
    },
    watch: {
//      'formModel': {
//        handler: function (val, oldVal) {
//        },
//        deep: true
//      },
//      'formModel.docType': function (val, oldVal) {
//        this.init(val)
//      }
    },
    components: {
      KalixOrgSelect: UserOrgSelect,
      KalixDictSelect: BaseDictSelect,
      KalixDialog: Dialog,
      Tinymce,
      KalixCheckbox: BaseCheckbox
    }
  }
</script>

<style scoped lang="stylus">

</style>
