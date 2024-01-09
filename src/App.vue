<template>
  <vue-cal
    class="vuecal--green-theme vuecal--rounded-theme " 
    locale="zh-cn"
    :selected-date="selectedDate"
    :disable-views="['years', 'year']"
    :time-from="8 * 60"
    :time-to="20 * 60"
    :events="events"
    @ready="fetchEvents" 
    @view-change="fetchEvents"
></vue-cal>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import VueCal from 'vue-cal'
import 'vue-cal/dist/vuecal.css'

const events = ref([])

const fetchEvents = ({ view, startDate, endDate, week }) => {
    console.log('Fetching events', { view, startDate, endDate, week })
    
    // Do an ajax call here with the given startDate & endDate.
    // Your API should return an array of events for this date range.      
    

    // Here we pretend an API call with a Promise and the setTimeout simulates the payload time.
    new Promise((resolve, reject) => { setTimeout(resolve, 400) })
      .then(() => {
        console.log('Events fetched, view:', view)
        events.value = [
          {
            start: '2024-01-10 10:00',
            end: '2024-01-10 11:00',
            title: '治疗',
            content: '<i class="icon material-icons">shopping_cart</i>',
            class: 'leisure'
          },
          {
            start: '2024-01-10 11:00',
            end: '2024-01-10 11:05',
            title: '预约',
            content: '<i class="icon material-icons">golf_course</i>',
            class: 'sport'
          },
          {
            start: '2024-01-11 11:00',
            end: '2024-01-11 11:10',
            title: '随访',
            content: '<i class="icon material-icons">golf_course</i>',
            class: 'sport'
          },
          {
            start: '2024-01-11 13:00',
            end: '2024-01-11 14:00',
            title: '治疗',
            content: '<i class="icon material-icons">shopping_cart</i>',
            class: 'leisure'
          },
          {
            start: '2024-01-12 15:00',
            end: '2024-01-12 15:10',
            title: '随访',
            content: '<i class="icon material-icons">cake</i>',
            class: 'sport'
          }
        ]
      });
  };

const selectedDate = ref(new Date());

const demoExample = ref({
  splits: [{ label: 'John', class: 'john' }, { label: 'Kate', class: 'kate' }],
  editable: { title: false, drag: true, resize: true, create: true, delete: true },
  events: []
});

</script>

<style>

#app {
  font-size: 14px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.vuecal {height: 90vh;}

.vuecal__event.leisure {background-color: rgba(253, 156, 66, 0.9); border: 1px solid rgb(233, 136, 46); color: #fff;}
.vuecal__event.sport {background-color: rgba(255, 102, 102, 0.9); border: 1px solid rgb(235, 82, 82); color: #fff;}

</style>
