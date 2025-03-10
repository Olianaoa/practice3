<script setup>
import login from './components/login.vue'
import test from './components/test.vue'
import all_pep from './components/all_pep.vue'
import modal from './components/modal.vue'

import { ref, reactive } from 'vue'

const local_data = reactive({
  you_here: 1,

  curUserIndex: null,
  users: [],

  modalShow: false,
  modalText: '',
  modalType: '',
})

function test_modal() {
  if (!(local_data.curUserIndex === null)) {
    local_data.you_here = 2;
  } else {
    local_data.modalShow = true
    local_data.modalText = 'Необходимо авторизоваться'
    local_data.you_here = 1
  }
}

function check_auth() {
  if (local_data.curUserIndex === null && local_data.you_here == 2) {
    local_data.you_here = 1;
    local_data.modalShow = true
    local_data.modalText = 'Выберите аккаунт, чтобы продолжить'
  }
}

// добавление информации пользователя в массив + пользователь становится активным 
function regist_user(params) {
  local_data.users.push(params)
  local_data.curUserIndex = local_data.users.length - 1;
}

// обновление результатов прохождения теста текущего пользователя
function test_results({ attempt, result }) {
  local_data.users[local_data.curUserIndex].user_attempts = attempt; //обновление кол-ва попыток
  local_data.users[local_data.curUserIndex].user_test_result = result;//обновление среднего результата
}


function delete_user(index_to_del) {
  //если индекс на удаление совпадает с индексом активного пользователя - в селекте появляется "Нет пользователя"
  if (index_to_del === local_data.curUserIndex) {
    local_data.curUserIndex = null;
  }
  // удаление пользователя из массива
  local_data.users.splice(index_to_del, 1);
}

</script>

<template>

  <div class="buttons">
    <button @click="local_data.you_here = 1"
      :class="{ 'you_here_style': local_data.you_here === 1 }">Авторизация</button>
    <button @click="test_modal" :class="{ 'you_here_style': local_data.you_here === 2 }">Тест</button>
    <button @click="local_data.you_here = 3" :class="{ 'you_here_style': local_data.you_here === 3 }">Все
      Пользователи</button>

    <select v-model="local_data.curUserIndex" @change="check_auth">
      <option :value=null>Нет пользователя</option>
      <option :value="index" v-for="(elem, index) in local_data.users">{{ elem.name }}</option>
    </select>
  </div>
  <!-- перенос информации о пользователе из компонента login.vue  -->
  <login v-if="local_data.you_here == 1" @login_Emit="regist_user" />

  <!-- переходим на страницу с тестом. В компонент test.vue отправляется информация о кол-ве попыток и текущем результате -->
  <test v-if="local_data.you_here == 2" 
  :us_attempt="local_data.users[local_data.curUserIndex].user_attempts"
  :us_result="local_data.users[local_data.curUserIndex].user_test_result" 
  @testEmit="test_results" />


  <div v-if="local_data.you_here == 3">
    <h2>Это страница со всеми авторизованными ниндзя</h2>
    <div class="grid_box">
      <!-- передача информации на страницу "Все пользователи" 
        elem - вся информация о пользователе
        index - индекс в массиве -->
      <all_pep v-for="(elem, index) in local_data.users" 
      :userData="elem" 
      :user_index="index"
      :active_user="local_data.curUserIndex" 
      @delete_Emit="delete_user" />
    </div>
  </div>

  <div>
    <modal v-if="local_data.modalShow" modalType="yes" 
    :modalText="local_data.modalText"
      @modalYes="local_data.modalShow = false" />
  </div>
</template>

<style scoped>
.you_here_style {
  border: orange double 5px;
}

.buttons {
  padding: 20px;
  justify-content: space-around;
  display: flex;
  min-width: 1280px;
}

.main {
  height: 400px;
  border: white solid 2px;
}

.grid_box {
  margin-top: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  gap: 10px;
}
</style>
