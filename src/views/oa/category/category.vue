<!--
描述：办公自动化-流程分类组件
开发人：sunlf
开发日期：2017年8月17日
-->

<template lang="pug">
  keep-alive
    kalix-table(bizKey="category" title='流程分类列表' v-bind:tableFields="tableFields"
      v-bind:targetURL="targetURL"
      v-bind:formModel.sync="formModel"
      v-bind:bizDialog="bizDialog"
      v-bind:bizSearch="'OaCategorySearch'"
      v-bind:btnList="btnList")
</template>

<script type="text/ecmascript-6">
  // import BaseTable from '@/components/custom/baseTable'
  // import {CategoryURL, OaCategoryComponent, ToolButtonList} from '../config.toml'
  import {CategoryURL, ToolButtonList} from '../config.toml'
  // import {registerComponent} from '@/api/register'

  // 注册全局组件
  // registerComponent(OaCategoryComponent)

  export default {
    name: 'kalix-oa-category',
    activated() {
      console.log(this.bizKey + '  is activated')
    },
    deactivated() {
      console.log(this.bizKey + '  is deactivated')
    },
    data() {
      return {
        btnList: ToolButtonList,
        targetURL: CategoryURL,
        tableFields: [
          {prop: 'name', label: '分类名称'},
          {prop: 'key', label: '分类主键'},
          {prop: 'icon', label: '分类图标'},
          {prop: 'description', label: '分类描述'}
        ],
        bizDialog: [
          {id: 'view', dialog: 'OaCategoryView'},
          {id: 'edit', dialog: 'OaCategoryAdd'},
          {id: 'add', dialog: 'OaCategoryAdd'}
        ],
        formModel: {
          name: '',
          key: '',
          icon: '',
          description: ''
        }
      }
    },
    created() {
//      this.tempFormModel = JSON.stringify(Object.assign({}, this.formModel))
    },
    mounted() {
    },
    filter: {},
    methods: {},
    components: {
      // BaseTable
//      KalixUserAdd: UserAdd
    }
  }
</script>

<style scoped lang="stylus">

</style>
