<!--
描述：办公自动化-流程分类-新增组件
开发人：sunlf
开发日期：2017年8月17日
-->

<template lang="pug">
  kalix-dialog.user-add(bizKey="category"
    ref="kalixBizDialog" v-bind:form-model="formModel" v-bind:targetURL="targetURL")
    div.el-form(slot="dialogFormSlot")
      el-form-item(label="分类名称" prop="name" v-bind:rules="rules.name")
        el-input(v-model="formModel.name")
      el-form-item(label="分类主键" prop="key" v-bind:rules="rules.key")
        el-input(v-model="formModel.key")
      el-form-item(label="分类图标" prop="icon")
        el-input(v-model="formModel.icon")
      el-form-item(label="分类描述" prop="description")
        el-input(v-model="formModel.description")
</template>

<script type="text/ecmascript-6">
  // import Dialog from '@/components/custom/baseDialog.vue'
  import {CategoryURL} from '../config.toml'

  export default {
    name: 'OaCategoryAdd',
    props: {
      formModel: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        rules: {
          name: [{required: true, message: '请输入分类名称', trigger: 'blur'}],
          key: [{required: true, message: '请输入分类主键', trigger: 'blur'}]
        },
        targetURL: CategoryURL
      }
    },
    created() {
    },
    components: {
      // KalixDialog: Dialog
    },
    methods: {}
  }
</script>

