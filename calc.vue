<template>
  <div>
    <div>历史记录</div>
    <el-table :data="historyData" style="width: 100%">
      <el-table-column prop="buy" label="购买价"/>
      <el-table-column prop="limit" label="限制"/>
      <el-table-column prop="reduce" label="减少" />
      <el-table-column prop="num" label="数量" />
      <el-table-column prop="b200" label="买200" />
      <el-table-column prop="b300" label="买300" />
      <el-table-column prop="b400" label="买400" />
      <el-table-column label="操作">
        <template #default="scope">
          <el-button link type="primary" size="small" @click="deleteItem(scope.$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <el-form :model="formData">
    <el-form-item label="购买价">
      <el-input type="number" v-model.number="formData.buy"/>
    </el-form-item>
    <el-form-item label="限制">
      <el-input type="number" v-model.number="formData.limit"/>
    </el-form-item>
    <el-form-item label="减少">
      <el-input type="number" v-model.number="formData.reduce"/>
    </el-form-item>
    <el-form-item label="数量">
      <el-input type="number" v-model.number="formData.num"/>
    </el-form-item>
  </el-form>
  <el-button type="primary" @click="calc">计算</el-button>
</template>

<script setup>
import { ref, reactive, toRefs, onMounted } from 'vue'

const historyData = ref([])
const formData = ref({
  buy: 168,
  limit: 300,
  reduce: 100,
  num: 3,
  b200: 0,
  b300: 0,
  b400: 0,
})


const calc = () => {
  const data1 = formData.value.limit * formData.value.num;
  const data2 = (formData.value.limit - formData.value.reduce) * formData.value.num + formData.value.buy;
  const percent = data2 / data1;
  formData.value.b200 = (percent * 200).toFixed(1);
  formData.value.b300 = (percent * 300).toFixed(1);
  formData.value.b400 = (percent * 400).toFixed(1);
  addItem(formData.value);
  historyData.value.push(Object.assign({}, formData.value));
}

const getItems = () => {
  return JSON.parse(localStorage.getItem('calc')) || []
}

const addItem = (item) => {
  const history = getItems()
  history.push(item)
  localStorage.setItem('calc', JSON.stringify(history))
}

const deleteItem = (index) => {
  historyData.value.splice(index, 1)
  localStorage.setItem('calc', JSON.stringify(historyData.value))
}

onMounted(() => {
  historyData.value = getItems()
})


</script>


