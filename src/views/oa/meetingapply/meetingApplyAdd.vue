<!--
描述：人员管理-会议申请-新增组件
开发人：sunlf
开发日期：2017年8月17日
-->
<template lang="pug">
  kalix-dialog.user-add(bizKey="meetapply"
    ref="kalixBizDialog" v-bind:formModel.sync="formModel" v-bind:targetURL="targetURL")
    div.el-form.kalix-form-table(slot="dialogFormSlot")
      div.table-title 吉林动画学院会议申请表
      div.s-flex
        el-form-item.s-flex_item(label="名称" v-bind:label-width="labelWidth"
        v-bind:rules="rules.title"
        prop="title")
          el-input(v-model="formModel.title")
      div.s-flex
        el-form-item.s-flex_item.kalix-form-table-td(label="申请部门" v-bind:label-width="labelWidth"
        v-bind:rules="rules.orgId"
        prop="orgId")
          kalix-org-select(v-model="formModel.orgId" v-on:selectChange="onOrgIdChange")
        el-form-item.s-flex_item(label="会议地点" v-bind:label-width="labelWidth"
        v-bind:rules="rules.meetingroomId"
        prop="meetingroomId")
          kalix-meeting-room-select(v-model="formModel.meetingroomId")
      div.s-flex
        el-form-item.s-flex_item(label="会议议题" v-bind:label-width="labelWidth"
        v-bind:rules="rules.meetingTopic"
        prop="meetingTopic")
          el-input(v-model="formModel.meetingTopic")
      div.s-flex
        el-form-item.s-flex_item(label="会议议程" v-bind:label-width="labelWidth"
        v-bind:rules="rules.meetingAgenda"
        prop="meetingAgenda")
          el-input(v-model="formModel.meetingAgenda")
      div.s-flex
        el-form-item.s-flex_item.kalix-form-table-td(
        label="开始时间"
        v-bind:label-width="labelWidth"
        v-bind:rules="rules.beginTime"
        prop="beginTime")
          kalix-datepicker-simple(v-model="formModel.beginTime" type="datetime" placeholder="选择开始时间" format="yyyy-MM-dd HH:mm" style="width: 100%;")
        el-form-item.s-flex_item.kalix-form-table-td(
        label="结束时间"
        v-bind:label-width="labelWidth"
        v-bind:rules="rules.endTime"
        prop="endTime")
          kalix-datepicker-simple(v-model="formModel.endTime" type="datetime" placeholder="选择结束时间" format="yyyy-MM-dd HH:mm" style="width: 100%;")
      div.s-flex
        el-form-item.s-flex_item.kalix-form-table-td(label="会议类型" v-bind:label-width="labelWidth")
          kalix-dict-select(v-model="formModel.meetingType" appName="oa" dictType="会议类型"
          style="width:100%")
        el-form-item.s-flex_item.kalix-form-table-td(label="会议纪要人员" v-bind:label-width="labelWidth")
          kalix-user-select(v-bind:params="params" style="width:100%"
          v-model="meetingSummaryPerson"
          v-bind:userIds.sync="formModel.meetingSummaryPerson"
          v-bind:userNames.sync="formModel.meetingSummaryPersonName"
          placeholder="请输入会议纪要人员")
      div
        el-form-item(label="重要出席人" v-bind:label-width="labelWidth")
          kalix-user-select(v-bind:params="params" style="width:100%"
          v-model="importantAttendees"
          v-bind:multiple="multiple"
          v-bind:userIds.sync="formModel.importantAttendees"
          v-bind:userNames.sync="formModel.importantAttendeesName"
          placeholder="请输入重要出席人")
      div
        el-form-item(label="其他出席人" v-bind:label-width="labelWidth")
          kalix-user-select(v-bind:params="params" style="width:100%"
          v-model="otherAttendees"
          v-bind:multiple="multiple"
          v-bind:userIds.sync="formModel.otherAttendees"
          v-bind:userNames.sync="formModel.otherAttendeesName"
          placeholder="请输入其他出席人")
</template>

<script type="text/ecmascript-6">
  // import Dialog from '@/components/custom/baseDialog.vue'
  // import BaseDictSelect from '@/components/custom/baseDictSelect'
  // import UserSelect from '@/components/biz/userselect/userselect'
  // import BaseDatePicker from '@/components/custom/baseDatePicker'
  import {MeetingApplyURL} from '../config.toml'
  import FormModel from './model'
  import UserOrgSelect from '../../../components/biz/select/UserOrgSelect'
  import MeetingRoomSelect from '../../../components/biz/select/MeetingRoomSelect'

  export default {
    name: 'OaMeetingApplyAdd',
    data() {
      return {
        myImportantAttendees: '',
        myImportantAttendeesIds: '',
        formModel: Object.assign({}, FormModel),
        params: {},
        title: '吉林动画学院会议申请表',
        rules: {
          title: [{required: true, message: '请输入名称', trigger: 'blur'}],
          beginTime: [{required: true, message: '请选择开始时间', trigger: 'change'}],
          endTime: [{required: true, message: '请选择结束时间', trigger: 'change'}],
          orgId: [{type: 'number', required: true, message: '请至少选择一个部门', trigger: 'change'}],
          meetingroomId: [{type: 'number', required: true, message: '请选择一个会议地点', trigger: 'change'}],
          meetingTopic: [{required: true, message: '请输入议题', trigger: 'blur'}],
          meetingAgenda: [{required: true, message: '请输入议程', trigger: 'blur'}]
        },
        targetURL: MeetingApplyURL,
        multiple: true,
        meetingSummaryPerson: null,
        importantAttendees: [],
        otherAttendees: []
      }
    },
    created() {
      this.labelWidth = '110px'
    },
    mounted() {
    },
    components: {
      // KalixDialog: Dialog,
      // KalixDictSelect: BaseDictSelect,
      // KalixUserSelect: UserSelect,
      // KalixDatePicker: BaseDatePicker,
      KalixOrgSelect: UserOrgSelect,
      KalixMeetingRoomSelect: MeetingRoomSelect
    },
    methods: {
      onOrgIdChange(item) {
        this.formModel.orgName = item.name
      }
    }
  }
</script>
