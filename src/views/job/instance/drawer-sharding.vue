<template>
  <el-drawer v-model="state.drawer.isShow" direction="rtl" size="50%" @close="onDrawerClose()">
    <template #header>
      <div>
        <h4>任务实例详情</h4>
      </div>
    </template>
    <template #default>
      <el-tabs  type="border-card" style="border-top: none;height: 100%;">
        <el-tab-pane label="基本信息">
          <el-descriptions column="1" border>
            <el-descriptions-item label="任务应用">openjob</el-descriptions-item>
            <el-descriptions-item label="任务名称">kooriookami</el-descriptions-item>
            <el-descriptions-item label="调度节点">kooriookami</el-descriptions-item>
            <el-descriptions-item label="分片参数">kooriookami</el-descriptions-item>
            <el-descriptions-item label="任务状态">成功</el-descriptions-item>
            <el-descriptions-item label="创建时间">2023-07-03 20:51:38</el-descriptions-item>
            <el-descriptions-item label="完成时间">2023-07-03 20:51:38</el-descriptions-item>
          </el-descriptions>
        </el-tab-pane>
        <el-tab-pane label="任务分片">
          <el-table :data="tableData" size="default" style="width: 100%">
            <el-table-column prop="workerAddress" label="执行机器" v-show="false"/>
            <el-table-column prop="taskStatus" label="任务状态" v-show="false"/>
            <el-table-column prop="createTIme" label="开始时间"  v-show="false" />
            <el-table-column prop="completeTime" label="完成时间"  v-show="false" />
            <el-table-column fixed="right" label="操作" width="120">
              <template #default>
                <el-button link type="primary" size="small" @click="handleClick">
                  详情
                </el-button>
                <el-button link type="danger" size="small">
                  终止
                </el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-tab-pane>
      </el-tabs>
    </template>
    <template #footer>
    </template>
  </el-drawer>
</template>

<script setup lang="ts" name="jobDrawerName">
import {useI18n} from "vue-i18n";
import {reactive} from "vue";
import {getInstanceStatusInfo} from "/@/utils/data";


const tableData = [
  {
    workerAddress: '172.20.0.253:25588',
    taskStatus: '成功',
    createTIme: '2023-07-03 20:51:41',
    completeTime: '2023-07-03 20:51:41',
  }
]

const {t} = useI18n();

// 定义接口
const state = reactive({
  drawer: {
    isShow: false
  },
  descriptions: {
    column: 6,
    workerAddress: '',
    createTime: '',
    completeTime: '',
    statusTag: '',
    statusLabel: 'Default',
  }
})

const openDrawer = async (row: RowJobInstanceType) => {
  // Description
  state.descriptions.workerAddress = row.workerAddress;
  state.descriptions.createTime = row.createTime;
  state.descriptions.completeTime = row.completeTime;
  state.descriptions.statusTag = getInstanceStatusInfo(row.status)['tag'];
  state.descriptions.statusLabel = getInstanceStatusInfo(row.status)['label'];

  state.drawer.isShow = true;
}

const onDrawerClose = () => {
}

// 暴露变量
defineExpose({
  openDrawer,
});
</script>
