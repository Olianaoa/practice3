<script setup>
import { ref, reactive } from 'vue'

const emit = defineEmits([
    'login_Emit'
])

const local_data = reactive({
    User_data: {
        inp_name: '',
        gender: '',
        inp_age: '',

        user_test_result: 0,
        user_attempts: 0,
    },

    error_mes_name: '',
    error_mes_gender: '',
    error_mes_age: '',
})


function continue_btn() {
    let test_inp_data = true
    if (/^[А-Я][а-я]+$/.test(local_data.User_data.inp_name)) {
        local_data.error_mes_name = ' '
    } else {
        local_data.error_mes_name = 'Ошибка! Только кириллица, с заглавной буквы'
        test_inp_data = false
    }
    if (local_data.User_data.gender != '') {
        local_data.error_mes_gender = ' '
    } else {
        local_data.error_mes_gender = 'Ошибка! Выберите пол'
        test_inp_data = false
    }
    if (local_data.User_data.inp_age > 4 && local_data.User_data.inp_age < 101) {
        local_data.error_mes_age = ''
    } else {
        local_data.error_mes_age = 'Ошибка! Авторизоваться могут только ниндзя от 5 до 100 лет'
        test_inp_data = false
    }

    if (test_inp_data) {
        emit('login_Emit', {
            name: local_data.User_data.inp_name,
            gender: local_data.User_data.gender,
            age: local_data.User_data.inp_age,
            user_test_result: local_data.User_data.user_test_result,
            user_attempts: local_data.User_data.user_attempts,
        });

        local_data.User_data.inp_name = '';
        local_data.User_data.gender = '';
        local_data.User_data.inp_age = '';
    }
}

</script>

<template>
    <h2>Создание аккаунта ниндзя</h2>
    <div class="login">
        <fieldset class="name">
            <legend>Введите имя (Только кириллица, с заглавной буквы)</legend>
            <input type="text" placeholder="Имя" v-model="local_data.User_data.inp_name">
            <div class="error">{{ local_data.error_mes_name }}</div>
        </fieldset>

        <fieldset class="gender">
            <legend>Выберите пол</legend>
            <input type="radio" name="gender" id="g1" v-model="local_data.User_data.gender" value="Мужской"> <label
                for="g1">Мужской
            </label><br>
            <input type="radio" name="gender" id="g2" v-model="local_data.User_data.gender" value="Женский"> <label
                for="g2">Женский
            </label><br>
            <input type="radio" name="gender" id="g3" v-model="local_data.User_data.gender" value="Не скажу"><label
                for="g3">Не
                скажу</label><br>
            <div class="error">{{ local_data.error_mes_gender }}</div>
        </fieldset>

        <fieldset class="age">
            <legend>Укажите возраст (от 5 до 100 лет)</legend>
            <input type="number" placeholder="Возраст" v-model="local_data.User_data.inp_age">
            <div class="error">{{ local_data.error_mes_age }}</div>
        </fieldset><br>
        <button @click="continue_btn">Войти</button><br>
    </div>

</template>

<style scoped>
.login {
    max-width: 600px;
    margin: 0 auto;
}

.error {
    color: red;
}
</style>