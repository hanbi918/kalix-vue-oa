<!--
描述：办公自动化-系统管理-文号管理
开发人：hqj-修改
开发日期：2018年01月10日
-->

<template lang="pug">
  keep-alive
    kalix-table(bizKey="document" title='文号列表' v-bind:targetURL="targetURL"
      v-bind:bizDialog="bizDialog" bizSearch="OaDocumentSearch" v-bind:btnList="btnList"
      v-bind:toolbarBtnList="toolbarBtnList" v-bind:customTableTool="customTableTool"
      v-bind:isFixedColumn="isFixedColumn" v-bind:dictDefine="dictDefine")
      template(slot="tableColumnSlot")
        kalix-biz-no-column(title="文号")  // 业务编号
        el-table-column(prop="docTypeName" label="文号类型" align="center" width="120")
        el-table-column(prop="year" label="年份" align="center" width="100")
        kalix-status-column  // 文号状态
        el-table-column(prop="title" label="文题" align="center" width="300")
        kalix-doc-status-column  // 文件状态
        el-table-column(prop="docDate" label="发文时间" align="center" width="220")
        el-table-column(prop="docDept" label="发文部门" align="center" width="220")
        kalix-date-column(prop="creationDate" label="创建时间")
        kalix-date-column(prop="updateDate" label="更新时间")
</template>

<script type="text/ecmascript-6">
  // import BaseTable from '@/components/custom/baseTable'
  import {DocumentURL, RedheadApplyURL, DocumentRevokeURL, DocumentAbolishURL} from '../config.toml'
  import {DocumentToolButtonList} from '../document/index'
  // import {registerComponent} from '@/api/register'
  import BizNoColumn from '../comp/bizNoColumn'
  import StatusColumn from '../comp/statusColumn.vue'
  import DocStatusColumn from '../comp/docStatusColumn.vue'
  // import DateColumn from 'views/oa/comp/dateColumn'
  // import Message from 'common/message'
  // import {ON_REFRESH_DATA} from '@/components/custom/event.toml'
  // import EventBus from 'common/eventbus'
  // import {baseURL} from 'config/global.toml'
  import {baseURL} from 'kalix-vue-common/src/config/global.toml'
  import redheadapplyFormModel from '../redheadapply/model'
  //  import {DictKeyValueObject} from 'common/keyValueObject'

  // 注册全局组件
  // registerComponent(DocumentComponent)
  // registerComponent(RedheadApplyComponent)

  export default {
    name: 'kalix-oa-document',
    data() {
      return {
        dictDefine: [{ // 定义数据字典的显示
          cacheKey: 'OA-DICT-KEY',
          type: '文号类型',
          targetField: 'docTypeName',
          sourceField: 'docType'
        }],
        isFixedColumn: true,
        btnList: DocumentToolButtonList,
        toolbarBtnList: [
          {id: 'add', isShow: false}
        ],
        targetURL: DocumentURL,
//        tableFields: [
//          {prop: 'docTypeName', label: '文号类型', width: '120'},
//          {prop: 'year', label: '年份', width: '100'},
//          {prop: 'businessNo', label: '文号', width: '200'},
//          {prop: 'status', label: '文号状态', width: '100'},
//          {prop: 'docDate', label: '发文时间', width: '160'},
//          {prop: 'docDept', label: '发文部门', width: '200'},
//          {prop: 'title', label: '文题', width: '280'},
//          {prop: 'creationDate', label: '创建时间', width: '160'},
//          {prop: 'updateDate', label: '更新时间', width: '160'}
//        ],
        bizDialog: [
          {id: 'view', dialog: 'OaDocumentView'},
          {id: 'publish', dialog: 'OaDocumentPublish'}
//          {id: 'preview', dialog: 'OaRedheadPreview'}
        ],
        redheadApplyURL: RedheadApplyURL,
        documentRevokeURL: DocumentRevokeURL,
        documentAbolishURL: DocumentAbolishURL,
        redheadapplyFormModel: Object.assign({}, redheadapplyFormModel)
      }
    },
    components: {
      // BaseTable,
      KalixBizNoColumn: BizNoColumn,
      KalixStatusColumn: StatusColumn,
      KalixDocStatusColumn: DocStatusColumn
      // KalixDateColumn: DateColumn
    },
    created() {
    },
    methods: {
      customTableTool(row, btnId, table) {
        switch (btnId) {
          // 撤回文号
          case 'revoke': {
            this.onRevoke(row, table)
            break
          }
          // 废除文号
          case 'abolish': {
            this.onAbolish(row, table)
            break
          }
          // 发文
          case 'publish': {
            this.onPublish(row, table)
            break
          }
          // 预览
          case 'preview': {
            this.onPreview(row, table)
            break
          }
          // 下载转成word
          case 'download': {
            this.onDownload(row, table)
            break
          }
        }
      },
      // 撤回文号
      onRevoke(row, table) {
        this.axios.request({
          method: 'GET',
          url: this.redheadApplyURL + '/' + row.redheadId,
          params: {}
        }).then((res) => {
          let docStatus = res.data.docStatus
          let warnInfo = '使用该文号的文件状态为[' + docStatus + '],确定要撤回该文号吗?'
          table.$confirm(warnInfo, '警告', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.axios.request({
              method: 'GET',
              url: this.documentRevokeURL + row.id,
              params: {}
            }).then((res) => {
              this.$KalixMessage.processResult(res)
              this.$KalixEventBus.$emit(this.$KalixEventConfig.ON_REFRESH_DATA)
            })
          })
        })
      },
      // 废除文号
      onAbolish(row, table) {
        this.axios.request({
          method: 'GET',
          url: this.redheadApplyURL + '/' + row.redheadId,
          params: {}
        }).then((res) => {
          let docStatus = res.data.docStatus
          let warnInfo = '使用该文号的文件状态为[' + docStatus + '],确定要废除该文号吗?'
          table.$confirm(warnInfo, '警告', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.axios.request({
              method: 'GET',
              url: this.documentAbolishURL + row.id,
              params: {}
            }).then((res) => {
              this.$KalixMessage.processResult(res)
              this.$KalixEventBus.$emit(this.$KalixEventConfig.ON_REFRESH_DATA)
            })
          })
        })
      },
      // 发文
      onPublish(row, table) {
        let dig =
          table.bizDialog.filter((item) => {
            return item.id === 'publish'
          })
        table.whichBizDialog = dig[0].dialog
        setTimeout(() => {
          row.other = '学校领导、学校各单位、部门'
          table.$refs.kalixDialog.$refs.kalixBizDialog.open('发文', true, row)
        }, 20)
      },
      // 预览
      onPreview(row, table) {
//        if (row.redheadId && (row.status === '使用中') && (row.docStatus === '审批通过'))
        if (row.redheadId) {
//          let url = RedheadApplyURL + '/' + row.redheadId
//          this.axios.request({
//            method: 'GET',
//            url: url,
//            params: {}
//          }).then(res => {
//            if (res.data.success === undefined) {
//              if (res.data) {
//                this.redheadapplyFormModel = res.data
//                // 处理红头文件标题
//                let _keyObj = DictKeyValueObject('OA-DICT-KEY', '文号标题')
//                this.redheadapplyFormModel.docCaption = _keyObj[this.redheadapplyFormModel.docType]
//                let dig =
//                  table.bizDialog.filter((item) => {
//                    return item.id === 'preview'
//                  })
//                table.whichBizDialog = dig[0].dialog
//                setTimeout(() => {
//                  table.$refs.kalixDialog.open(this.redheadapplyFormModel)
//                }, 20)
//              }
//            }
//          })
          window.open(baseURL + '/camel/servlet/download?beanname=RedheadApply&id=' + row.redheadId + '&filetype=html')
        } else {
          this.$KalixMessage.warning('文号未关联红头文件,无法进行预览!')
        }
      },
      // 下载转成word
      onDownload(row, table) {
        window.open(baseURL + '/camel/servlet/download?beanname=RedheadApply&id=' + row.redheadId + '&filetype=word')
      }
    }
  }
</script>

<style scoped lang="stylus">

</style>
