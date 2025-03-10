<script setup>
import { ref, reactive, computed } from 'vue';
import modal from './modal.vue';

const local = reactive({
    modalShow: false,
    modalType: '',
    user_to_delete: '',
});

const props = defineProps({
    userData: '',
    user_index: '',
    active_user: '',
});

const emit = defineEmits([
    'delete_Emit',
]);

function deleteUser(index) {
    emit('delete_Emit', index)
    local.modalShow = false
}

</script>

<template>
    <div class="box" :class="{ 'active_user': props.user_index === props.active_user }"   >
        
        Индекс: {{ props.user_index }} <br>
        Имя: {{ props.userData.name }}<br>
        Пол: {{ props.userData.gender }}<br>
        Возраст: {{ props.userData.age }}<br>
        Средний результат теста: {{ props.userData.user_test_result }}<br>
        Попытки прохождения: {{ props.userData.user_attempts }}<br>

        <button @click="local.modalShow = true; local.user_to_delete = props.user_index;">
            Удалить пользователя
        </button>
    </div>

    <modal v-if="local.modalShow" modalType="answer" 
    :modalText="'Вы точно хотите удалить пользователя?'"
        @modalYes="deleteUser(props.user_index)"
        @modalNo="local.modalShow = false" />

</template>

<style scoped>
.box {
    min-width: 200px;
    border: solid 2px rgb(22, 9, 65);
    border-radius: 10%;
    margin: 10px;
    padding: 10px;
    text-align: left;
}

.active_user {
    border: orange double 10px;
}
</style>