<!--
描述：办公自动化-流程分类-查詢组件
开发人：sunlf
开发日期：2017年8月17日
-->

<template lang="pug">
  kalix-search(title="流程分类查询"
    v-bind:searchFields="searchFields" v-bind:form-rules="rules")
</template>

<script type="text/ecmascript-6">
  // import BaseSearch from '@/components/custom/baseSearch.vue'

  export default {
    name: 'OaCategorySearch',
    data() {
      return {
        searchFields: [
          {label: '分类名称', prop: 'name'}
        ],
        rules: {
          name: [{required: true, message: '请输入分类名称', trigger: 'blur'}]
        }
      }
    },
    created() {
    },
    components: {
      // KalixSearch: BaseSearch
    },
    methods: {}
  }
</script>

<style scoped lang="stylus" type="text/stylus">
</style>
