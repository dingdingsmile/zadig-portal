<template>
  <div class="side-menu">
    <ul>
      <li>
        <span class="title">基本信息</span>
        <span class="operation"></span>
      </li>
      <li>
        <a @click="addModule('buildDeploy')"
           href="javascript:"
           class="title">构建部署
          <i class="operation el-icon-circle-plus-outline"></i>
        </a>
      </li>
      <li>
        <a @click="addModule('artifactDeploy')"
           href="javascript:"
           class="title">交付物部署
          <i class="operation el-icon-circle-plus-outline"></i>
        </a>
      </li>
      <li>
        <a @click="addModule('test')"
           href="javascript:"
           class="title">测试
          <i class="operation el-icon-circle-plus-outline"></i>
        </a>
      </li>
      <li>
        <a @click="addModule('distribute')"
           href="javascript:"
           class="title">分发部署
          <i class="operation el-icon-circle-plus-outline"></i>
        </a>
      </li>
       <li>
        <a @click="addModule('extension')"
           href="javascript:"
           class="title">扩展
          <i class="operation el-icon-circle-plus-outline"></i>
        </a>
      </li>
      <li>
        <a @click="addModule('trigger')"
           href="javascript:"
           class="title">触发器
          <i class="operation el-icon-circle-plus-outline"></i>
        </a>
      </li>
      <li>
        <a @click="addModule('notify')"
           href="javascript:"
           class="title">通知
          <i class="operation el-icon-circle-plus-outline"></i>
        </a>
      </li>
    </ul>
  </div>
</template>

<script type="text/javascript">
import mixin from '@/mixin/workflowMixin'
export default {
  data () {
    return {
      tabCheckResult: {}
    }
  },
  methods: {
    addModule (module_name) {
      this.checkCurrentTab().then(() => {
        this.$emit('add-module', module_name)
      })
    }
  },
  props: {
    currentTab: {
      required: true,
      type: String
    },
    products: Array
  },
  mixins: [mixin]
}
</script>

<style lang="less">
.side-menu {
  display: flex;
  margin: auto 0;
  overflow: hidden;
  color: rgba(0, 0, 0, 0.87);
  background-color: #fff;
  border-top-right-radius: 6px;
  border-bottom-right-radius: 6px;
  box-shadow:
    0 1px 6px rgba(0, 0, 0, 0.117647),
    0 1px 4px rgba(0, 0, 0, 0.117647);

  ul {
    margin: 0;
    padding: 15px;
  }

  ul > li {
    display: flex;
    flex-direction: column;
    padding: 10px 0;

    .title {
      display: inline-block;
      color: #0e1726;
      font-size: 14px;
      line-height: 24px;
      text-align: left;

      .operation,
      .lock-menu {
        font-size: 14px;
        cursor: pointer;
      }
    }

    a.title:hover {
      color: @themeColor;
    }
  }
}
</style>
