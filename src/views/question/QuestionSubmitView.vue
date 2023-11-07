<template>
  <div id="questionSubmitView">
    <a-form :model="searchParams" layout="inline">
      <a-form-item field="questionId" label="题号" style="min-width: 240px">
        <a-input v-model="searchParams.questionId" placeholder="请输入题号" />
      </a-form-item>
      <a-form-item field="language" label="编程语言" style="min-width: 240px">
        <a-select
          v-model="searchParams.language"
          :style="{ width: '320px' }"
          placeholder="请选择编程语言"
        >
          <a-option>java</a-option>
          <a-option>c</a-option>
          <a-option>cpp</a-option>
          <!-- <a-option>go</a-option> -->
        </a-select>
      </a-form-item>
      <a-form-item>
        <a-button type="primary" @click="doSubmit">搜索</a-button>
      </a-form-item>
      <a-form-item>
        <a-button
          type="primary"
          shape="round"
          status="success"
          @click="loadData"
          >刷新
        </a-button>
      </a-form-item>
    </a-form>
    <a-divider size="0" />
    <a-table
      :ref="tableRef"
      :columns="columns"
      :data="dataList"
      :pagination="pagination"
      @page-change="onPageChange"
    >
      <template #judgeInfo="{ record }">
        {{ JSON.stringify(record.judgeInfo) }}
      </template>
      <template #createTime="{ record }">
        {{ moment(record.createTime).format("YYYY-MM-DD HH:mm:ss") }}
      </template>
    </a-table>
  </div>
</template>

<script setup lang="ts">
import { onMounted, reactive, ref, watchEffect } from "vue";
import {
  QuestionControllerService,
  QuestionSubmitQueryRequest,
} from "../../../generated";
import message from "@arco-design/web-vue/es/message";
import moment from "moment";

const tableRef = ref();

const dataList = ref([]);
const total = ref<number>(0);
const searchParams = ref<QuestionSubmitQueryRequest>({
  questionId: undefined,
  language: undefined,
  pageSize: 10,
  current: 1,
});
const pagination = reactive({
  ...searchParams.value,
  showTotal: true,
  total: total,
  showJumper: true,
});
const onPageChange = (current: number) => {
  pagination.current = current;
  searchParams.value.current = current;
  loadData();
};
const loadData = async () => {
  const res =
    await QuestionControllerService.listQuestionSubmitViewByPageUsingPost({
      ...searchParams.value,
      sortField: "createTime",
      sortOrder: "descend",
    });
  if (res.code === 0) {
    dataList.value = res.data.records;
    total.value = res.data.total;
  } else {
    message.error("加载失败，" + res.message);
  }
};

/**
 * 监听 searchParams 变量， 改变时触发页面的重新加载
 */
watchEffect(() => {
  loadData();
});

/**
 * 页面加载时，请求数据
 */
onMounted(() => {
  loadData();
});

const columns = [
  {
    title: "提交号",
    dataIndex: "id",
    width: 200,
  },
  {
    title: "编程语言",
    dataIndex: "language",
    width: 120,
  },
  // {
  //   title: "判题信息",
  //   dataIndex: "judgeInfo",
  //   slotName: "judgeInfo",
  //   width: 300,
  // },
  {
    title: "判题信息",
    dataIndex: "judgeInfo",
    children: [
      {
        title: "信息",
        dataIndex: "message",
        width: 150,
      },
      {
        title: "耗时",
        dataIndex: "time",
        width: 150,
      },
    ],
  },
  {
    title: "题目 ID",
    dataIndex: "questionId",
  },
  {
    title: "提交者 ID",
    dataIndex: "userId",
  },
  {
    title: "创建时间",
    dataIndex: "createTime",
    slotName: "createTime",
    width: 170,
  },
];

const doSubmit = () => {
  searchParams.value = {
    ...searchParams.value,
    current: 1,
  };
};
</script>

<style scoped>
#questionSubmitView {
  max-width: 1280px;
  margin: 0 auto;
}
</style>
