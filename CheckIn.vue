<template>
  <div>
    <div v-if="!successMessage">
      <p>欢迎{{ username }}，请签到</p>
      <button class="check-in-button" @click="checkIn">签到</button>
    </div>
    <div v-if="successMessage">
      <p>{{ currentDay.year }}年{{ currentDay.month }}月{{ currentDay.date }}日签到成功</p>
    </div>
    <div v-if="successMessage">
      <div class="body">
        <table>
          <thead>
            <tr>
              <th>Sun</th>
              <th>Mon</th>
              <th>Tue</th>
              <th>Wed</th>
              <th>Thu</th>
              <th>Fri</th>
              <th>Sat</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(week, index) in weeks" :key="'week-' + index">
              <td v-for="(day, index) in week" :key="day ? (day.date + '-' + day.month) : ('empty-' + index)" :class="{ today: isToday(day), checked: isChecked(day), 'current-day': isCurrentDay(day) }" @click="checkIn(day)">
                 <div class="date" v-if="day">{{ day.date }}</div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentDate: new Date(),
      checkedDays: [],
      showCalendar: false,
      successMessage: false,
      currentDay:[]
    };
  },
  mounted() {
    this.initCalendar();
    this.currentDay = this.getCurrentDay();
  },
  methods: {
    initCalendar() {
      const now = new Date();
      const year = now.getFullYear();
      const month = now.getMonth() + 1;
      const firstDay = new Date(year, month - 1, 1);
      const lastDay = new Date(year, month, 0);
      const firstWeekDay = firstDay.getDay();
      const lastWeekDay = lastDay.getDay();
      const days = lastDay.getDate();
      const weeks = [];
      let week = [];

      for (let i = 0; i < firstWeekDay; i++) {
        week.push(null);
      }

      for (let i = 1; i <= days; i++) {
        week.push({ date: i, month });
        if ((i + firstWeekDay) % 7 === 0 || i === days) {
          weeks.push(week);
          week = [];
        }
      }

      for (let i = lastWeekDay; i < 6; i++) {
        week.push(null);
      }

      this.title = `${year}年${month}月`;
      this.weeks = weeks;
    },
    getCurrentDay() {
      const now = new Date();
      const year = now.getFullYear();
      const month = now.getMonth() + 1;
      const date = now.getDate();
      this.currentDay = { year, month, date };
      return { year, month, date };
    },
    isToday(day) {
  if (!day) {
    return false;
  }
  const now = new Date();
  const year = now.getFullYear();
  const month = now.getMonth() + 1;
  const date = now.getDate();
  return day.year === year && day.month === month && day.date === date;
},
isChecked(day) {
  if (!day) {
    return false;
  }
  return this.checkedDays.some(item => item.year === day.year && item.month === day.month && item.date === day.date);
},
isCurrentDay(day) {
  if (!day) {
    return false;
  }
  const now = new Date();
  const year = now.getFullYear();
  const month = now.getMonth() + 1;
  const date = now.getDate();
  return day.year === year && day.month === month && day.date === date && !this.isChecked(day);
},

checkIn(day) {
if (!day) {
return;
}
if (!this.isChecked(day)) {
this.checkedDays.push(day);
this.successMessage = true;
} else {
alert('今天已经签到了');
}
},

},
computed: {
username() {
return 'User';
}
}
};
</script>

<style scoped>
.date {
  font-size: 14px;
}
table {
  width: 100%;
}
th,
td {
  text-align: center;
  padding: 10px 0;
  width: 14.28%;
  position: relative;
  border: 1px solid #ccc;
  cursor: pointer;
}
.today::before {
  content: '';
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 6px;
  height: 6px;
  background: #1890ff;
  border-radius: 50%;
}
.checked {
  background: #1890ff;
  color: #fff;
}
.check-in-button {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
  width: 100px;
  height: 100px;
  background-color: #42b983;
  color: #fff;
  border-radius: 50%;
  cursor: pointer;
  font-size: 20px;
}


</style>