<template>
    <h2>My Calendar</h2>
    <div id="changeLang">
        <button @click="changeLang('en')">en</button>
        <button @click="changeLang('ru')">ru</button>
    </div>
    <div id="container">
        <div id="calHeader">
            <button class="monthSwitchBtn" id="prevMonth" @click="prevMonth(selectedDate)">←</button>
            <span>{{ selectedMonthName }} {{ selectedDate.getFullYear() }}</span>
            <button class="monthSwitchBtn" id="nextMonth" @click="nextMonth">→</button>
        </div>
        <div id="weekdays">
            <span v-for="n in WEEKDAYS[currentLang]" :key="n">{{ n.slice(0, 3) }}</span>
        </div>
        <div id="days">
            <div v-for="n in getFirstDayOfMonth(selectedDate)" :key="n"></div>
            <button
                :class="{ day: true, selected: n === initDate.getDate() && compareMonthAndYear(initDate, selectedDate) }"
                @click="onDayClick" v-for="n in getDaysOfMonth(selectedDate)" :key="n">{{ n }}</button>
        </div>
    </div>
</template>

<script setup>
import { defineProps, onMounted, ref, computed, defineEmits } from 'vue';


const props = defineProps(["initDate"]);
const initDate = ref(new Date())
const selectedDate = ref(new Date())
const emits = defineEmits(["chosenDate"])

onMounted(() => {
    if (!isNaN(props.initDate)) {
        initDate.value = new Date(props.initDate);
        selectedDate.value = new Date(props.initDate);
    }
})

const WEEKDAYS = {
    en: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday",],
    ru: ["Воскресенье","Понедельник","Вторник","Среда","Четверг","Пятница","Суббота",]
}
const MONTHS = {
    en: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
    ru: ["Январь","Февраль","Март","Апрель","Май","Июнь","Июль","Август","Сентябрь","Октябрь","Ноябрь","Декабрь",]
}

const selectedMonthName = computed(() => MONTHS[currentLang.value][selectedDate.value.getMonth()].slice(0, 3))
const currentLang = ref("en")

const prevMonth = () => {
    addMonth(-1)
}


const nextMonth = () => {
    addMonth(1)
}

const addMonth = (n) => {
    setDate(selectedDate.value, 1)
    selectedDate.value = new Date(selectedDate.value.setMonth(selectedDate.value.getMonth() + n))
}

const setDate = (date, n) => {
    date.setDate(n)
}

const compareMonthAndYear = (date1, date2) => {
    return date1.getMonth() === date2.getMonth() && date1.getFullYear() === date2.getFullYear()
}

const onDayClick = (e) => {
    setDate(selectedDate.value, e.target.innerHTML)
    let res = `${selectedDate.value.getFullYear()}-${selectedDate.value.getMonth() + 1}-${selectedDate.value.getDate()}`
    emits("chosenDate", res)
}

const getDaysOfMonth = (date) => {
    return new Date(date.getFullYear(), date.getMonth() + 1, 0).getDate()
}

const getFirstDayOfMonth = (date) => {
    return new Date(date.getFullYear(), date.getMonth(), 1).getDay()
}

const changeLang = (targetLang) => {
    currentLang.value = targetLang
}
</script>


<style scoped>
#container {
    height: auto;
    width: 250px;
    border: 1px solid black;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin: auto;
    padding: 3px;
}

#calHeader {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

#weekdays,
#days {
    font-size: 0.7rem;
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 3px;
}

.day,
.monthSwitchBtn {
    height: 2rem;
    width: 2rem;
    background-color: transparent;
    cursor: pointer;
    border: 1px solid transparent;
}

.day:focus,
.monthSwitchBtn:hover {
    border: 1px solid #4ebfdf;
}

.selected {
    border: 1px solid #ff0000;
}

#changeLang {
    display: flex;
    justify-content: center;
    gap: 1rem;
    padding: 1rem;
}
</style>