<template>
  <div id="app">
    <Calendar :checked="checked" :clickHandler="clickHandler"/>
  </div>
</template>

<script>
  import Calendar from '@/components/calendar'

export default {
  name: 'App',
  components: {
    Calendar
  },
  data() {
    return{
      checked: ["20190101","20190305","20190211"]
    }
  },
  methods: {
    clickHandler(day) {
      if(day.checked){
        this.checked.splice(this.checked.indexOf(day.value), 1);
      } else {
        this.checked.push(day.value)
      }
    }
  }
}
</script>
