<!--
描述：办公自动化-会议室申请-查询组件
开发人：sunlf
开发日期：2017年8月17日
-->

<template lang="pug">
  kalix-search(title="会议室申请查询"
    v-bind:searchFields="searchFields" v-bind:form-rules="rules")
</template>

<script type="text/ecmascript-6">
  // import BaseSearch from '@/components/custom/baseSearch.vue'

  export default {
    name: 'OaMeetingroomApplySearch',
    data() {
      return {
        searchFields: [
          {label: '申请部门', prop: 'orgName'},
          {label: '经办人', prop: 'host'}
        ],
        rules: {
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
