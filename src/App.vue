<template>
  <div>
    <Calendar :date="selectedDate" @selected="onSelected"/>
    <div class="selected-date">Выбранная дата: "{{selectedDate}}"</div>
  </div>
</template>

<script>
import Calendar from "@/components/Calendar.vue";
import { ref } from "vue";
export default {
  components: {
    Calendar
  },
  setup() {
    const selectedDate = ref("")

    const onSelected = (date) => {
      selectedDate.value = date
    }

    return {
      selectedDate,
      onSelected
    }
  }
};
</script>

<style>
.selected-date {
  margin: 22px auto;
  width: 300px;
  display: block;
  padding: 8px 16px;
  border-radius: 4px;
  background-color: #f8f9fa;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  font-family: Arial, sans-serif;
  font-size: 16px;
}
</style>