<template lang="pug">
  div.el-form.kalix-form-table(slot="dialogFormSlot")
    div.table-title 吉林动画学院用章申请表
    el-form-item(label="名称" v-bind:label-width="labelWidth")
      el-input(v-model="formModel.title")
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="申请部门" v-bind:label-width="labelWidth")
        kalix-org-select(v-model="formModel.orgId" v-on:selectChange="onOrgIdChange")
      el-form-item.s-flex_item.kalix-form-table-td(label="用印数" v-bind:label-width="labelWidth")
        el-input-number(v-model="formModel.usageCount" v-bind:min="1" v-bind:max="10" style="width:100%")
    el-form-item(label="印章类别" v-bind:label-width="labelWidth")
      kalix-dict-select(v-model="formModel.sealType" appName="oa" dictType="印章类型")
    el-form-item(label="备注" v-bind:label-width="labelWidth")
      el-input(v-model="formModel.remark")
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="部门负责人" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.departmentHead" readonly)
      el-form-item.s-flex_item.kalix-form-table-td(label="分公司负责人" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.tableFormField" readonly)
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="法律顾问" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.counsel"  readonly)
      el-form-item.s-flex_item.kalix-form-table-td(label="总经理" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.generalManager" readonly)
    div.s-flex
      el-form-item.s-flex_item.kalix-form-table-td(label="印章专管员" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.sealAdministrator" readonly)
      el-form-item.s-flex_item.kalix-form-table-td(label="经办人" v-bind:label-width="labelWidth")
        el-input(v-model="formModel.createBy" readonly)
</template>

<script type="text/ecmascript-6">
  import {SealApplyURL} from '../config.toml'
  import UserOrgSelect from '@/components/biz/select/UserOrgSelect'
  import Dialog from '@/components/custom/baseDialog.vue'
  import BaseDictSelect from '@/components/custom/baseDictSelect'

  export default {
    props: ['formModel'],
    created() {
      this.labelWidth = '110px'
    },
    data() {
      return {
        targetURL: SealApplyURL,
        rules: {
          title: [{required: true, message: '请输入名称', trigger: 'blur'}]
        }
      }
    },
    methods: {
      onOrgIdChange(item) {
        this.formModel.orgName = item.name
      }
    },
    components: {
      KalixOrgSelect: UserOrgSelect,
      KalixDictSelect: BaseDictSelect,
      KalixDialog: Dialog
    }
  }
</script>

<style scoped lang="stylus">

</style>
