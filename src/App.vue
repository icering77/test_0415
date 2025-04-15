<template>
  <div class="container d-flex flex-wrap">
    <div class="col-12 col-md-6 p-3">
      <div class="border rounded p-3">
        <h2>操作</h2>
        <!-- <pre>{{ users }}</pre> -->
        <!-- <pre>{{ users[(users.length-1)].id }}</pre> -->
        <form @submit.prevent>
          <BFormControls
              class="mb-3"
              label="名字"
              v-model="formDate.name"/>

          <BFormControls
              class="mb-3"
              label="年齡"
              type="number"
              value="formDate.age"
              v-model="formDate.age"/>

          <div class="d-flex gap-1">
            <button class="btn btn-success" @click="edit">修改</button>
            <button class="btn btn-warning" @click="create">新增</button>
          </div>
        </form>
      </div>
    </div>

    <div class="col-12 col-md-6 p-3">
      <div class="border rounded p-3">
        <table class="table">
          <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">名字</th>
            <th scope="col">年齡</th>
            <th scope="col">操作</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(v,i) in users" :key="`user_${i}`">
            <th scope="row">{{ v.id }}</th>
            <td>{{ v.name }}</td>
            <td>{{ v.age }}</td>
            <td class="d-flex gap-1">
              <button class="btn btn-success" @click="selectUser(v)">
                修改
              </button>
              <button class="btn btn-danger" @click="remove(v)">
                刪除
              </button>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import axios from 'axios'
import {onMounted, ref} from 'vue';
import BFormControls from "./components/BFormControls.vue";

interface User {
  id: number;
  name: string;
  age: number;
}

const baseUrl = 'https://41133.wu.elitepro.ltd' // 由面試官提供
const users = ref<User[]>([])
const formDate = ref({
  // id readonly
  id: 0,
  name: '',
  age: 0,
})


const create = () => {
  // 需有確認步驟
  if (formDate.value.name === '') {
    alert('請輸入姓名及年齡');
  } else if ( formDate.value.age === 0 ) {
    alert('請輸入姓名及年齡');
  } else{

    const alertText = '確定新增' + formDate.value.name + '(' + formDate.value.age + ') 嗎?'
    const result = confirm(alertText)
    if (result) {
      const userId = users.value[(users.value.length-1)].id + 1

      users.value.push({
        id: userId,
        name: formDate.value.name,
        age: formDate.value.age
      })

    } else {
    }
  } 

}

const edit = () => {
  // 需有確認步驟
  const alertText = '確定修改' + formDate.value.name + '(' + formDate.value.age + ') 嗎?'
  const result = confirm(alertText)
  if (result) {
    const userIndex = users.value.findIndex(item => item.id === formDate.value.id)
    users.value[userIndex].name = formDate.value.name
    users.value[userIndex].age = formDate.value.age
    

  } else {
  }
}


const selectUser = (user: User) => {
  // 禁止使用 formDate.value = user
  formDate.value.name = user.name
  formDate.value.age = user.age
  formDate.value.id = user.id
  
}

const remove = (user: User) => {
  // 需有確認步驟
  const alertText = '確定刪除嗎?'
  const result = confirm(alertText)
  if (result) {
    users.value = users.value.filter(item => item.id !== user.id)
  } else {
  }
}

const getUsers = () => {
  axios({
    method: 'get',
    url: baseUrl + '/api/user',
  }).then(res => {
    const {data} = res.data
    users.value = data
  }).catch(err => {
    console.log(err)
  })

}

const setupPage = () => {
  getUsers()
}

onMounted(setupPage)
</script>

<style scoped>

</style>
