<template>
  <h1>Made by Temur KHOSHIMOV</h1>
  <MyCalendar :init-date=initDate v-on:chosen-date="calendarResult"></MyCalendar>
  <br>
  {{ calendarResponse }}
</template>

<script setup>
import { ref } from 'vue';
import MyCalendar from './components/MyCalendar.vue'

const initDate = ref("2024-01-31")
const calendarResponse = ref()

const calendarResult = (val)=>{
  calendarResponse.value = val
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
