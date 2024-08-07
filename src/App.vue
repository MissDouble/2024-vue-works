<script setup>
import { ref } from 'vue'
const menu = ref([
  { id: 0, name: '珍珠奶茶', description: '香濃奶茶搭配QQ珍珠', price: 50, storage: 20 },
  { id: 1, name: '冬瓜檸檬', description: '清新冬瓜配上新鮮檸檬', price: 45, storage: 18 },
  { id: 2, name: '翡翠檸檬', description: '綠茶與檸檬的完美結合', price: 55, storage: 34 },
  { id: 3, name: '四季春茶', description: '香醇四季春茶，回甘無比', price: 45, storage: 10 },
  { id: 4, name: '阿薩姆奶茶', description: '阿薩姆紅茶搭配香醇鮮奶', price: 50, storage: 25 },
  { id: 5, name: '檸檬冰茶', description: '檸檬與冰茶的清新組合', price: 45, storage: 20 },
  { id: 6, name: '芒果綠茶', description: '芒果與綠茶的獨特風味', price: 55, storage: 18 },
  { id: 7, name: '抹茶拿鐵', description: '抹茶與鮮奶的絕配', price: 60, storage: 20 }
])

const addOne = (item) => {
  item.storage += 1
}

const minusOne = (item) => {
  if (item.storage > 0) {
    item.storage -= 1
  }
}

const isEditing = ref(false)

const tempEdit = ref({
  id: 0,
  name: '',
  description: '',
  price: 0,
  storage: 0
})

const prepareEdit = (item) => {
  tempEdit.value = { ...item }
  isEditing.value = true
}

const confirmEdit = () => {
  const index = menu.value.findIndex((item) => item.id === tempEdit.value.id)
  if (!tempEdit.value.name) {
    alert('品項不能為空白')
    return
  } else if (!tempEdit.value.description) {
    alert('描述不能為空白')
    return
  } else if (tempEdit.value.price === '') {
    alert('價格不能為空白')
    return
  } else if (tempEdit.value.storage === '') {
    alert('庫存不能為空白')
    return
  }
  menu.value[index] = { ...tempEdit.value }
  resetEdit()
}

const resetEdit = () => {
  tempEdit.value = { id: 0, name: '', description: '', price: 0, storage: 0 }
  isEditing.value = false
}
</script>

<template>
  <div class="container mt-4 mb-5">
    <h1 class="text-center mb-4">六角學院 2024 Vue 前端新手營 - Composition API</h1>
    <h2 class="text-center mb-4">作業：week 1</h2>
    <hr />
    <table class="table table-bordered table-striped">
      <thead class="table-dark">
        <tr>
          <th scope="col">品項</th>
          <th scope="col">描述</th>
          <th scope="col">價格</th>
          <th scope="col">庫存</th>
          <th scope="col">操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in menu" :key="item.id">
          <td>{{ item.name }}</td>
          <td>
            <small>{{ item.description }}</small>
          </td>
          <td>${{ item.price }}</td>
          <td>
            <div class="d-flex justify-content-between align-items-center">
              <button
                class="btn btn-secondary btn-sm"
                @click="minusOne(item)"
                :disabled="item.storage < 1"
              >
                -1
              </button>
              {{ item.storage }}
              <button class="btn btn-secondary btn-sm" @click="addOne(item)">+1</button>
            </div>
          </td>
          <td>
            <button type="button" class="btn btn-primary btn-sm" @click="prepareEdit(item)">
              編輯
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <hr />
    <div v-if="isEditing" class="container mt-4">
      <h2 class="text-center mb-4">編輯區域</h2>
      <div class="row g-3">
        <div class="col-md-4">
          <label for="edit-name" class="form-label">品項</label>
          <input id="edit-name" type="text" class="form-control" v-model="tempEdit.name" />
        </div>
        <div class="col-md-4">
          <label for="edit-description" class="form-label">描述</label>
          <input
            id="edit-description"
            type="text"
            class="form-control"
            v-model="tempEdit.description"
          />
        </div>
        <div class="col-md-2">
          <label for="edit-price" class="form-label">價格</label>
          <input
            id="edit-price"
            type="number"
            class="form-control"
            v-model.number="tempEdit.price"
            min="0"
          />
        </div>
        <div class="col-md-2">
          <label for="edit-storage" class="form-label">庫存</label>
          <input
            id="edit-storage"
            type="number"
            class="form-control"
            v-model.number="tempEdit.storage"
            min="0"
          />
        </div>
        <div class="col-12 text-center">
          <button type="button" class="btn btn-success" @click="confirmEdit">確認編輯</button>
          <button type="button" class="btn btn-secondary ms-2" @click="resetEdit">取消編輯</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
