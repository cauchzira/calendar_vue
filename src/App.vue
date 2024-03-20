<template>
  <div>
    <Calendar :date="selectedDate" @selected="onSelected"/>
    <div>Выбранная дата: "{{selectedDate}}"</div>
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