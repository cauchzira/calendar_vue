<template>
  <div class="calendar">
    <div class="header">
      <button @click="prevMonth">&lt</button>
      <span>{{currentMonth}} {{ currentDate.getFullYear() }}</span>
      <button @click="nextMonth">&gt</button>
    </div>
    <div class="weekdays">
      <div v-for="(day, index) in weekdays" :key="day+index" class="weekday">{{day}}</div>
    </div>
    <div class="days">
      <div class="day" v-for="(day, index) in days" @click="selectDate(day)" :class="{'selected': isSelected(day)}" :key="day+index">{{day}}</div>
    </div>
  </div>
</template>

<script>
import {computed, onMounted, ref} from "vue";

export default {
  name: "Calendar",
  props: {
    language: {
      type: String,
      default: ""
    },
    date: {
      type: String,
      default: ""
    }
  },
  setup(props, {emit}) {
    const currentDate = ref(props.date ? new Date(props.date) : new Date())
    const currentLanguage = computed(() => {
      const lang =  props.language || navigator.language

      if (lang.startsWith("ru")) {
        return "ru";
      } else if (lang.startsWith("en")) {
        return "en";
      }

      return "en";
    })
    const selectedDate = ref(null)

    const months = {
      en: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
      ru: ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"]
    }

    const daysOfWeek = {
      en: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
      ru: ["Вс", "Пн", "Вт", "Ср", "Чт", "Пт", "Сб"]
    }

    const currentMonth = computed(() => {
      return months[currentLanguage.value][currentDate.value.getMonth()]
    })

    const weekdays = computed(() => {
      return daysOfWeek[currentLanguage.value]
    })


    const days = computed(() => {
      const year = currentDate.value.getFullYear();
      const month = currentDate.value.getMonth();
      const daysIsMonth = new Date(year, month + 1, 0).getDate();
      const startDatOfWeek = new Date(year, month, 1).getDay();
      const days = []

      for (let i = 1; i <= daysIsMonth; i++) {
        days.push(i);
      }

      for(let i = 0; i < startDatOfWeek; i++) {
        days.unshift('')
      }

      return days
    })


    const prevMonth = () => {
      currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() - 1, 1);
    }

    const nextMonth = () => {
      currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() + 1,  1);
    }

    const selectDate = (day) => {
      if(day) {
        selectedDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth(), day)
        emit('selected', selectedDate.value.toLocaleDateString())
      }
    }

    const isSelected = day => {
      return selectedDate.value && day === selectedDate.value.getDate();
    }

    onMounted(() => {
      selectDate(currentDate.value.getDate())
    })

    return {
      currentDate,
      currentLanguage,
      currentMonth,
      selectedDate,
      months,
      weekdays,
      daysOfWeek,
      days,

      prevMonth,
      nextMonth,
      selectDate,
      isSelected,
    }
  }
};
</script>

<style>
.calendar {
  font-family: Arial, sans-serif;
  max-width: 300px;
  margin: 0 auto;
  padding: 20px;
  border-radius: 8px;
  background-color: #f8f9fa;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.header button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 18px;
  color: #007bff;
  transition: transform 0.3s ease-in-out;
}

.header button:hover {
  transform: scale(1.1);
}

.weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  margin-bottom: 10px;
}

.weekday {
  padding: 5px;
  text-align: center;
  color: #495057;
}

.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day {
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  text-align: center;
  transition: background-color 0.3s ease-in-out;
}

.day:hover, .selected {
  background-color: #007bff;
  color: #fff;
}
</style>