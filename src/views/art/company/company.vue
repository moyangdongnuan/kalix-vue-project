<!--
描述：艺术中心-系统管理-公司信息
开发人：hqj
开发日期：2017年12月13日
-->

<template lang="pug">
  keep-alive
    base-table(bizKey="artCompany" title='公司信息列表' v-bind:targetURL="targetURL"
    v-bind:bizDialog="bizDialog" v-bind:tableFields="tableFields" bizSearch="ArtCompanySearch"
    v-bind:btnList="btnList" v-bind:toolbarBtnList="toolbarBtnList" v-bind:customTableTool="customTableTool"
    v-bind:dictDefine="dictDefine" v-bind:isFixedColumn="isFixedColumn")
</template>

<script type="text/ecmascript-6">
  import BaseTable from '@/components/custom/baseTable'
  import {CompanyURL, ArtCompanyComponent, ToolButtonCompanyList} from '../config.toml'
  import {registerComponent} from '@/api/register'

  // 注册全局组件
  registerComponent(ArtCompanyComponent)

  export default {
    data() {
      return {
        dictDefine: [{ // 定义数据字典的显示
          cacheKey: 'ART-DICT-KEY',
          type: '企业性质',
          targetField: 'natureName',
          sourceField: 'nature'
        }, {
          cacheKey: 'ART-DICT-KEY',
          type: '省份',
          targetField: 'regionName',
          sourceField: 'region'
        }],
        btnList: ToolButtonCompanyList,
        toolbarBtnList: [
          {id: 'add', isShow: false}
        ],
        targetURL: CompanyURL,
        tableFields: [
          {prop: 'code', label: '企业组织机构代码'},
          {prop: 'name', label: '企业名称'},
          {prop: 'natureName', label: '企业性质'},
          {prop: 'regionName', label: '所在省份'},
          {prop: 'city', label: '所在城市'},
          {prop: 'email', label: '邮箱'},
          {prop: 'phone', label: '固定电话'},
          {prop: 'address', label: '企业详细地址'}
        ],
        bizDialog: [
          {id: 'view', dialog: 'ArtCompanyView'},
          {id: 'newEdit', dialog: 'ArtCompanyAdd'}
        ],
        isFixedColumn: true
      }
    },
    components: {
      BaseTable
    },
    created() {
    },
    methods: {
      customTableTool(row, btnId, that) {
        switch (btnId) {
          case 'newEdit' : { // 新编辑
            that.whichBizDialog = ''
            let dig =
              that.bizDialog.filter((item) => {
                return item.id === 'newEdit'
              })
            that.whichBizDialog = dig[0].dialog
            setTimeout(() => {
              if (row.id && row.id > 0) { // 修改
                that.$refs.kalixDialog.$refs.kalixBizDialog.open('编辑', true, row)
              } else { // 新增
                this.$delete(row, 'version')
                this.$delete(row, 'version_')
                that.$refs.kalixDialog.$refs.kalixBizDialog.open('编辑', false, row)
              }
            }, 20)
            break
          }
        }
      }
    }
  }
</script>

<style scoped lang="stylus">

</style>
