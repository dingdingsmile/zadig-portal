<template>
  <div style="width: 100%; height: 250px;">
    <el-table :data="tableData" v-loading="loading" :show-header="true" class="build-table">
      <el-table-column prop="taskId" label="任务号">
        <template slot-scope="scope">
          <router-link
            class="task-link"
            :to="`/v1/projects/detail/${scope.row.productName}/pipelines/multi/${scope.row.pipelineName}/${scope.row.taskId}?status=${scope.row.status}`"
          >{{scope.row.pipelineName}}#{{scope.row.taskId}}</router-link>
        </template>
      </el-table-column>
      <el-table-column prop="maxDuration" width="100px" label="时长">
        <template slot-scope="scope">{{ $utils.timeFormat(scope.row.maxDuration) }}</template>
      </el-table-column>
      <el-table-column prop="createTime" label="日期">
        <template slot-scope="scope">
          <span class="date-info">{{ $utils.convertTimestamp(scope.row.createTime) }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="status" width="60px" label="状态">
        <template slot-scope="scope">
          <el-tag
            effect="dark"
            size="mini"
            :type="$utils.taskElTagType(scope.row.status)"
            close-transition
          >{{ wordTranslation(scope.row.status,'pipeline','task') }}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
import { getLongestBuildsAPI } from '@api'
import { wordTranslate } from '@utils/wordTranslate'
export default {
  data () {
    return {
      tableData: [],
      loading: true
    }
  },
  methods: {
    getLongestBuilds () {
      this.loading = true
      const startTime = Math.floor(this.selectedDuration[0] / 1000)
      const endTime = Math.floor(this.selectedDuration[1] / 1000)
      const selectedProjects = this.selectedProjects
      getLongestBuildsAPI({
        startDate: startTime,
        endDate: endTime,
        projectNames: selectedProjects
      }).then(res => {
        this.tableData = res
        this.loading = false
      })
    },
    wordTranslation (word, category, subitem) {
      return wordTranslate(word, category, subitem)
    }
  },
  watch: {
    selectedProjects: {
      handler () {
        this.getLongestBuilds()
      },
      immediate: false
    },
    selectedDuration: {
      handler () {
        this.getLongestBuilds()
      },
      immediate: false
    }
  },
  mounted () {
    this.getLongestBuilds()
  },
  props: {
    selectedProjects: {
      required: true
    },
    selectedDuration: {
      required: true
    }
  }
}
</script>

<style lang="less">
.build-table {
  .task-link {
    color: @themeColor;
  }

  .task-link,
  .date-info {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }

  width: 100%;
  font-size: 12px;

  &.el-table {
    th,
    td {
      padding: 0 !important;
    }
  }
}
</style>
