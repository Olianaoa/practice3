<script setup>
import { reactive } from 'vue';
import modal from './modal.vue'

const local = reactive({
    result_modal: 0, // результтат теста в модальное окно
    total_result: 0,
    count_user_attempts: 0,
    result_props: 0, // результат существующий из App
    medium_res: 0,

    modalShow: false,
    modalShow2: false,
    modalType: '',

    q1_Naruto_lastname: '',
    q2_team_number: 0,
    q3_Kurama: '',
    q4_Love: '',
    q5_dog_name: '',
    q6_Kakashi: 0,
    q7_Tsunade: '',
    q8_animals: 0,
    q9_hobby: '',
    q10: '',
})
const props = defineProps({
    us_attempt: Number,
    us_result: Number,
});

const emit = defineEmits([
    'testEmit'
])

function check_answ() {
    let result_in_function = 0;

    if (local.q1_Naruto_lastname == '' &&
        local.q2_team_number == 0 &&
        local.q3_Kurama == '' &&
        local.q4_Love == '' &&
        local.q5_dog_name == 0 &&
        local.q6_Kakashi == 0 &&
        local.q7_Tsunade == '' &&
        local.q8_animals == 0 &&
        local.q9_hobby == '' &&
        local.q10 == '') {

        local.modalShow = true;

    } else {
        result_in_function += Number(local.q1_Naruto_lastname)

        if (local.q2_team_number == 7) {
            result_in_function += 1
        }
        if (/[Кк]урама/.test(local.q3_Kurama)) {
            result_in_function += 1
        }
        if (/[Лл]юбовь/.test(local.q4_Love)) {
            result_in_function += 1
        }

        result_in_function += Number(local.q5_dog_name)
        result_in_function += Number(local.q6_Kakashi)

        if (/[Цц]унаде/.test(local.q7_Tsunade)) {
            result_in_function += 1
        }
        if (local.q8_animals == 9) {
            result_in_function += 1
        }

        result_in_function += Number(local.q9_hobby)

        if (/[Шш]икамару/.test(local.q10)) {
            result_in_function += 1 // результат за эту попытку теста (текущий)
        }

        local.modalShow2 = true;
        local.modalType = 'answer_saved'
    }
    local.result_modal = result_in_function;// для модального окна, сколько баллов за эту попытку прохождения теста

    local.count_user_attempts = props.us_attempt + 1; //счетчик попыток +1
    local.result_props = props.us_result;// существующий результат прохождения теста
    local.total_result = result_in_function + local.result_props; // (общий) текущий результат + cуществующий рез 
    local.medium_res = local.total_result / local.count_user_attempts;//среднее значение 

    //очищение полей ввода
    local.q1_Naruto_lastname = '';
    local.q2_team_number = 0;
    local.q3_Kurama = '';
    local.q4_Love = '';
    local.q5_dog_name = '';
    local.q6_Kakashi = 0;
    local.q7_Tsunade = '';
    local.q8_animals = 0;
    local.q9_hobby = '';
    local.q10 = '';
}
function send_result() {
    emit('testEmit', { attempt: local.count_user_attempts, result: local.medium_res });
}
</script>

<template>
    <h2>Пройди тест на знание аниме "Наруто"</h2>
    <div class="questions">
        <fieldset class="test">
            <legend>1. Какая фамилия у Наруто?</legend>
            <input type="radio" name="q1_Naruto_lastname" id="q1_Naruto_lastname_1" v-model="local.q1_Naruto_lastname"
                value="1">
            <label for="q1_Naruto_lastname_1">Узумаки</label><br>
            <input type="radio" name="q1_Naruto_lastname" id="q1_Naruto_lastname_2" v-model="local.q1_Naruto_lastname"
                value="0">
            <label for="q1_Naruto_lastname_2">Иванов </label><br>
            <input type="radio" name="q1_Naruto_lastname" id="q1_Naruto_lastname_3" v-model="local.q1_Naruto_lastname"
                value="00">
            <label for="q1_Naruto_lastname_3">Намиказе</label>
        </fieldset>

        <fieldset class="test">
            <legend>2. Под каким номером была команда Какаши Хатаке, в который состоял Наруто?</legend>
            <input type="number" placeholder="Введите сюда ответ" v-model="local.q2_team_number">
        </fieldset>

        <fieldset class="test">
            <legend>3. Как зовут Девятихвостого Лиса, запечатанного в Наруто?</legend>
            <input type="text" placeholder="Введите ответ" v-model="local.q3_Kurama">
            <!-- [Кк]урама -->
        </fieldset>

        <fieldset class="test">
            <legend>4. Что означает кандзи на левой стороне лба у Гаары?</legend>
            <input type="text" placeholder="Введите ответ" v-model="local.q4_Love">
            <!-- [Лл]юбовь -->
        </fieldset>

        <fieldset class="test">
            <legend>5. Как звали боевого пса Кибы Инузуки?</legend>
            <input type="radio" name="q5_dog_name" id="q5_dog_name_1" v-model="local.q5_dog_name" value="0">
            <label for="q5_dog_name_1">Шикамару</label><br>
            <input type="radio" name="q5_dog_name" id="q5_dog_name_2" v-model="local.q5_dog_name" value="1">
            <label for="q5_dog_name_2">Акамару </label><br>
            <input type="radio" name="q5_dog_name" id="q5_dog_name_3" v-model="local.q5_dog_name" value="00">
            <label for="q5_dog_name_3">Бобик</label><br>
            <input type="radio" name="q5_dog_name" id="q5_dog_name_4" v-model="local.q5_dog_name" value="000">
            <label for="q5_dog_name_4">Кусамару</label>
        </fieldset>

        <fieldset class="test">
            <legend>6. Какое прозвище имел Какаши Хатаке?</legend>
            <input type="radio" name="q6_Kakashi" id="q6_Kakashi_1" v-model="local.q6_Kakashi" value="000">
            <label for="q6_Kakashi_1">У него не было прозвища</label> <br>

            <input type="radio" name="q6_Kakashi" id="q6_Kakashi_2" v-model="local.q6_Kakashi" value="00000">
            <label for="q6_Kakashi_2">Белый Клык Конохи</label><br>
            <input type="radio" name="q6_Kakashi" id="q6_Kakashi_3" v-model="local.q6_Kakashi" value="0000000">
            <label for="q6_Kakashi_3">Бобик</label><br>
            <input type="radio" name="q6_Kakashi" id="q6_Kakashi_4" v-model="local.q6_Kakashi" value="000000001">
            <label for="q6_Kakashi_4">Копирующий ниндзя</label>
        </fieldset>

        <fieldset class="test">
            <legend>7. Как зовут 5 Хокаге деревни Скрытого Листа?</legend>
            <input type="text" placeholder="Введите ответ" v-model="local.q7_Tsunade">
            <!-- [Цц]унаде -->
        </fieldset>

        <fieldset class="test">
            <legend>8. Сколько всего хвостатых зверей существует?(После деления десятихвостого)</legend>
            <input type="number" placeholder="Введите сюда ответ" v-model="local.q8_animals">
        </fieldset>

        <fieldset class="test">
            <legend>9. Каким видом творчества занимался Джирайя?</legend>
            <input type="radio" name="q9_hobby" id="q9_hobby_1" v-model="local.q9_hobby" value="0">
            <label for="q9_hobby_1">Никаким, он бездельник</label> <br>
            <input type="radio" name="q9_hobby" id="q9_hobby_2" v-model="local.q9_hobby" value="00">
            <label for="q9_hobby_2">Писал картины</label> <br>
            <input type="radio" name="q9_hobby" id="q9_hobby_3" v-model="local.q9_hobby" value="1">
            <label for="q9_hobby_3">Писал книги</label> <br>
            <input type="radio" name="q9_hobby" id="q9_hobby_4" v-model="local.q9_hobby" value="000">
            <label for="q9_hobby_4">Читал рэп</label> <br>
        </fieldset>

        <fieldset class="test">
            <legend>10. Кто из сверстников Наруто первым сдал экзамен на звание чунина? (Только имя)</legend>
            <input type="text" placeholder="Введите ответ" v-model="local.q10">
            <!-- [Шш]икамару  -->
        </fieldset><br>

        <button @click="check_answ">проверить</button>
    </div>

    <div>
        <modal v-if="local.modalShow" modalType="answer"
            :modalText="'Вы не ответили ни на один вопрос. Вы уверены, что хотите завершить тест?'"
            @modalYes="local.modalShow = false,local.modalShow2=true, send_result()" @modalNo="local.modalShow = false" />
        <modal v-if="local.modalShow2" modalType="answer_saved"
            :modalText="'Результаты теста записаны. Вы ответили правильно на столько вопросов:'"
            :ModalTestResult="local.result_modal" @modalYes="local.modalShow2 = false, send_result()" />
    </div>
</template>

<style scoped>
.questions {
    max-width: 600px;
    display: flex;
    flex-direction: column;
    margin: 0 auto;
    text-align: left;
}

button {
    margin: 0 auto;
    align-items: center;
}

legend {
    font-weight: bold;
}

.test {
    margin: 15px;
}
</style>