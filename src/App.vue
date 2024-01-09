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

const url = 'http://127.0.0.1:8000/gemba/tasks_list/'

const events = ref([])

const fetchEvents = async ({ view, startDate, endDate, week }) => {
    console.log('Fetching events', { view, startDate, endDate, week })
    
    // Do an ajax call here with the given startDate & endDate.
    // Your API should return an array of events for this date range.      
    // The events should be in the following format:
    // {
    //   start: '2016-12-08 10:30', // Start date/time
    //   end: '2016-12-08 12:30',   // End date/time
    //   title: 'My nice event',    // Event title
    //   content: '<i class="icon material-icons">shopping_cart</i>', // Event content (HTML allowed)
    //   class: 'leisure'           // Event class (string, optional)
    // }
    
    try {
        // 发送请求到API
        const response = await fetch(`${url}?startDate=${startDate}&endDate=${endDate}`);
        
        // 检查响应状态
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        // 解析JSON响应
        const data = await response.json();

        // 更新events的响应式引用
        events.value = data.map(event => ({
          // 根据你的API返回结构映射到你的事件数据格式
          // 例如:
          // start: event.start,
          // end: event.end,
          // title: event.title,
          // content: event.content,
          // class: event.class
        }));
    } catch (error) {
        console.error('Error fetching events:', error);
    }

    // // Here we pretend an API call with a Promise and the setTimeout simulates the payload time.
    // new Promise((resolve, reject) => { setTimeout(resolve, 400) })
    //   .then(() => {
    //     console.log('Events fetched, view:', view)
    //     events.value = [
    //       {
    //         start: '2024-01-10 10:00',
    //         end: '2024-01-10 11:00',
    //         title: '治疗',
    //         content: '<i class="icon material-icons">shopping_cart</i>',
    //         class: 'leisure'
    //       },
    //       {
    //         start: '2024-01-10 11:00',
    //         end: '2024-01-10 11:05',
    //         title: '预约',
    //         content: '<i class="icon material-icons">golf_course</i>',
    //         class: 'sport'
    //       },
    //       {
    //         start: '2024-01-11 11:00',
    //         end: '2024-01-11 11:10',
    //         title: '随访',
    //         content: '<i class="icon material-icons">golf_course</i>',
    //         class: 'sport'
    //       },
    //       {
    //         start: '2024-01-11 13:00',
    //         end: '2024-01-11 14:00',
    //         title: '治疗',
    //         content: '<i class="icon material-icons">shopping_cart</i>',
    //         class: 'leisure'
    //       },
    //       {
    //         start: '2024-01-12 15:00',
    //         end: '2024-01-12 15:10',
    //         title: '随访',
    //         content: '<i class="icon material-icons">cake</i>',
    //         class: 'sport'
    //       }
    //     ]
    //   });
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
