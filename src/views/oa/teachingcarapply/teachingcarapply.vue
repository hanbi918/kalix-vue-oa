<!--
描述：办公自动化-教学用车申请
开发人：hqj
开发日期：2018年03月23日
-->
<template lang="pug">
  div
    keep-alive
      kalix-table(bizKey='teachingCarApply' title='教学用车申请列表' v-bind:targetURL='targetURL'
      v-bind:bizDialog='bizDialog' bizSearch='OaTeachingCarApplySearch' v-bind:btnList='btnList'
      v-bind:isFixedColumn="isFixedColumn" v-bind:customTableTool="customTableTool" v-bind:customRender="customRender")
        template(slot="tableColumnSlot")
          el-table-column(prop="title" label="名称" align="center" width="220")
          el-table-column
          kalix-biz-no-column // 业务编号
          el-table-column(prop="currentNode" label="当前环节" align="center" width="220")
          kalix-process-status-column // 工作流状态
          el-table-column(prop="auditResult" label="审批结果" align="center" width="220")
          el-table-column(prop="reason" label="用车事由" align="center" width="220")
          el-table-column(prop="usageCount" label="乘车人数" align="center" width="220")
          el-table-column(prop="address" label="用车起始地点" align="center" width="220")
          el-table-column(prop="cityName" label="市内用车" align="center" width="220")
          el-table-column(prop="orgName" label="申请部门" align="center" width="220")
          kalix-date-column(prop="creationDate" label="创建时间")
          kalix-date-column(prop="applyDate" label="申请时间")
          el-table-column(prop="createBy" label="经办人" align="center" width="90")
    kalix-task-view(ref="kalixDialog")
</template>

<script type="text/ecmascript-6">
  // import BaseTable from '@/components/custom/baseTable'
  // import {TeachingCarApplyURL, TeachingCarApplyComponent, TeachingCarApplyStartURL} from '../config.toml'
  import {TeachingCarApplyURL, TeachingCarApplyStartURL} from '../config.toml'
  // import {registerComponent} from '@/api/register'
  // import {workflowBtnList, registerComp, customTableTool} from '@/views/oa/comp'
  import {workflowBtnList, customTableTool} from '../comp'
  import BizNoColumn from '../comp/bizNoColumn'
  // import DateColumn from 'views/oa/comp/dateColumn'
  import ProcessStatusColumn from '../comp/processStatusColumn.vue'
  import TaskView from '../comp/taskView'

  // 注册全局组件
  // registerComponent(TeachingCarApplyComponent)

  export default {
    name: 'kalix-oa-teachingcarapply',
    data() {
      return {
        isFixedColumn: true,
        hasTableSelection: true,
        btnList: workflowBtnList,
        targetURL: TeachingCarApplyURL,
        bizDialog: [
          {id: 'view', dialog: 'OaTeachingCarApplyView'},
          {id: 'edit', dialog: 'OaTeachingCarApplyAdd'},
          {id: 'add', dialog: 'OaTeachingCarApplyAdd'},
          {id: 'progress', dialog: 'OaTaskView'}
        ]
      }
    },
    components: {
      // BaseTable,
      KalixBizNoColumn: BizNoColumn,
      // KalixDateColumn: DateColumn,
      KalixProcessStatusColumn: ProcessStatusColumn, // 工作流状态列
      KalixTaskView: TaskView
    },
    created() {
    },
    mounted() {
      // registerComp()
    },
    methods: {
      customTableTool(row, btnId) {
        customTableTool(row, btnId, TeachingCarApplyStartURL, this)
      },
      customRender(_data) {
        _data.forEach(function (e) {
          e.cityName = e.city ? '是' : '否'
        })
      }
    }
  }
</script>

<style scoped lang="stylus">
</style>
