<template>
  <div class="home-wrapper">
    <div class="table-container">
      <a-table
        :columns="columns"
        :data-source="data"
        :pagination="{ pageSize: 10 }"
        :scroll="{ y: 480 }"
        row-key="id"
        bordered
        size="middle"
      >
        <template #bodyCell="{ column, record }">
          <template v-if="column.key === 'overtime'">
            <a-tag :color="record.overtime ? 'red' : 'green'">
              {{ record.overtime ? '是' : '否' }}
            </a-tag>
          </template>
          <template v-if="column.key === 'action' && role === 'admin'">
            <a-button type="link" danger @click="handleDelete(record.id)">删除</a-button>
          </template>
        </template>
      </a-table>
      <!-- <div class="charts"></div> -->
      <Charts :data="data" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { data as sourceData } from '../data';
import Charts from './charts.vue';

const role = localStorage.getItem('username') === 'admin' ? 'admin' : 'user';


const data = ref([...sourceData]);

const handleDelete = (id: string) => {
  // 删除数据
  data.value = data.value.filter((item) => item.id !== id);
};

interface DataItem {
  id: string;
  project: string;
  overtime: boolean;
  hours: number;
  created_at: string;
}

const columns = [
  {
    title: 'ID',
    dataIndex: 'id',
    key: 'id',
    width: 80,
  },
  {
    title: '项目名称',
    dataIndex: 'project',
    key: 'project',
    width: 200,
  },
  {
    title: '加班',
    dataIndex: 'overtime',
    key: 'overtime',
    width: 100,
  },
  {
    title: '工时',
    dataIndex: 'hours',
    key: 'hours',
    width: 100,
    sorter: (a: DataItem, b: DataItem) => a.hours - b.hours,
  },
  {
    title: '创建时间',
    dataIndex: 'created_at',
    key: 'created_at',
    width: 180,
    sorter: (a: DataItem, b: DataItem) =>
      new Date(a.created_at).getTime() - new Date(b.created_at).getTime(),
  },
  {
    title: '操作',
    key: 'action',
    width: 80,
  },
];

</script>

<style scoped lang="less">
.home-wrapper {
  padding: 24px;
  min-height: 100vh;
  background: #f5f5f5;
}

.table-container {
  background: #fff;
  padding: 24px;
  border-radius: 6px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}

.charts {
  margin-top: 24px;
  height: 400px;
}
</style>
