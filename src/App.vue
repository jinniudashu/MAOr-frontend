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

// 服务器API地址
const baseURL = import.meta.env.VITE_MAOR_API_URL;
const url = `${baseURL}/gemba/tasks_list/`;

const events = ref([])

const fetchEvents = async ({ view, startDate, endDate, week }) => {
    
    // Do an ajax call here with the given startDate & endDate.
    // Your API should return an array of events for this date range.      
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
        events.value = data.map(event => {
            // 解析并格式化开始时间
            const startDateTime = new Date(event.schedule_time);
            const startFormatted = `${startDateTime.getFullYear()}-${(startDateTime.getMonth() + 1).toString().padStart(2, '0')}-${startDateTime.getDate().toString().padStart(2, '0')} ${startDateTime.getHours().toString().padStart(2, '0')}:${startDateTime.getMinutes().toString().padStart(2, '0')}`;

            // 计算并格式化结束时间（假设事件持续1小时）
            const endDateTime = new Date(startDateTime.getTime() + 60 * 60 * 1000);
            const endFormatted = `${endDateTime.getFullYear()}-${(endDateTime.getMonth() + 1).toString().padStart(2, '0')}-${endDateTime.getDate().toString().padStart(2, '0')} ${endDateTime.getHours().toString().padStart(2, '0')}:${endDateTime.getMinutes().toString().padStart(2, '0')}`;

            const title = event.name;
            const content = `客户编号: ${event.customer}, 任务编号: ${event.task}, 状态: ${event.status}`;

            // 根据状态设置类名
            const class_name = event.status === 'Pending' ? 'pending-class' : 'other-class';

          return {
            start: startFormatted,
            end: endFormatted,
            title: title,
            content: content,
            class: class_name
          }
        });

        console.log("events:", events.value)

    } catch (error) {
        console.error('Error fetching events:', error);
    }

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
