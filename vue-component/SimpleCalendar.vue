<template>
  <div class="calendar-card">
    <div class="calendar-bar">
      <button class="button button-year-minus" @click="minusYear(true)">
        &lt;
      </button>
      <button class="button button-month-minus" @click="minusMonth">
        &lt;
      </button>
      <div class="calendar-date">
        {{ date.year }}-{{ date.month + 1 }}-{{ date.date }}
      </div>
      <button class="button button-month-plus" @click="plusMonth">&gt;</button>
      <button class="button button-year-plus" @click="plusYear(true)">
        &gt;
      </button>
    </div>
    <div class="calendar-content">
      <ul class="ul-week">
        <li class="li-week" :v-for="item in week">{{ item }}</li>
      </ul>
      <ul class="ul-day">
        <li
          class="li-day"
          :v-for="item in days"
          :isThisMonth="item.isThisMonth"
          @click="handleClick(item)"
        >
          {{ item.date }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      date: {
        year: "",
        month: "",
        date: "",
      },
      current: {
        year: "",
        month: "",
        date: "",
      },
      week: ["日", "一", "二", "三", "四", "五", "六"],
      days: [],
    };
  },
  methods: {
    handleClick: function (item) {
      console.log(item.year + "-" + item.month + "-" + item.date);
      alert(item.year + "-" + item.month + "-" + item.date);
    },
    plusMonth: function () {
      if (this.date.month == 11) {
        this.date.month = 0;
        this.plusYear(false);
      } else {
        this.date.month++;
      }
      this.createCalendar(this.date.year, this.date.month);
    },
    minusMonth: function () {
      if (this.date.month == 0) {
        this.date.month = 11;
        this.minusYear(false);
      } else {
        this.date.month--;
      }
      this.createCalendar(this.date.year, this.date.month);
    },
    plusYear: function (create) {
      if (this.date.year == 2049) {
        this.date.year = 1970;
      } else {
        this.date.year++;
      }
      if (create) {
        this.createCalendar(this.date.year, this.date.month);
      }
    },
    minusYear: function (create) {
      if (this.date.year == 1970) {
        this.date.year = 2049;
      } else {
        this.date.year--;
      }
      if (create) {
        this.createCalendar(this.date.year, this.date.month);
      }
    },
    createCalendar: function (year, month) {
      let d = new Date();
      d.setFullYear(year);
      d.setMonth(month);
      d.setDate(1);
      let dayOfFirstDay = d.getDay();
      this.days = [];

      for (let i = 0; i < 42; i++) {
        d.setDate(1);
        d.setMonth(month);
        d.setDate(i - dayOfFirstDay + 1);

        let isThisMonth = 1;
        if (d.getMonth() == month) {
          isThisMonth = 2;
        } else {
          isThisMonth = 1;
        }

        if (
          this.current.date == d.getDate() &&
          this.current.month == d.getMonth() &&
          this.current.year == d.getFullYear()
        ) {
          isThisMonth = 3;
          let date = {
            year: year,
            month: month,
            date: d.getDate(),
            isThisMonth: isThisMonth,
          };
          this.days.push(date);
        } else {
          let date = {
            year: d.getFullYear(),
            month: d.getMonth(),
            date: d.getDate(),
            isThisMonth: isThisMonth,
          };

          this.days.push(date);
        }
      }
    },
  },
  created() {
    let d = new Date();
    this.date.year = d.getFullYear();
    this.date.month = d.getMonth();
    this.date.date = d.getDate();

    this.current.year = d.getFullYear();
    this.current.month = d.getMonth();
    this.current.date = d.getDate();

    this.createCalendar(this.current.year, this.current.month);
  },
};
</script>

<style scoped>
body {
  background-color: #ddd;
}

.calendar-card {
  position: absolute;
  /* width: 290px; */
  height: 220px;
  padding: 5px 5px 0px 5px;
  border: 1px solid #ebeef5;
  border-radius: 10px;
  background-color: #fff;
}

.calendar-card:hover {
  box-shadow: 0px 0px 5px 5px #cfcfcf;
}

.calendar-bar {
  height: 40px;
  color: #727272;
  line-height: 40px;
  text-align: center;
  /* background-color: #ebeef5; */
}

.calendar-date {
  display: inline-block;
  width: 100px;
}

.button {
  width: 20px;
  height: 20px;
  background-color: #fff;
  color: #aaa;
  border: 0;
  border-radius: 5px;
}

.button:hover {
  background-color: #eee;
  cursor: pointer;
}

.ul-week {
  display: grid;
  width: 245px;
  grid-template-rows: 20px;
  grid-template-columns: repeat(7, 35px); /**/
  list-style: none;
  text-align: center;
  font-size: 13px;
  color: #aaa;
  margin: 5px 20px 5px 20px;
  padding-inline-start: 0px;
  padding-inline-end: 0px;
  border-bottom: 1px solid #eee;
}

.li-week {
  display: inline-block;
}

.ul-day {
  display: grid;
  width: 245px;
  grid-template-rows: repeat(6, 22px);
  grid-template-columns: repeat(7, 35px);
  list-style: none;
  text-align: center;
  margin: 5px 20px 5px 20px;
  padding-inline-start: 0px;
  padding-inline-end: 0px;
}

.li-day {
  display: inline-block;
  font-size: 10px;
  line-height: 22px;
  text-align: center;
  border-radius: 5px;
  margin: 1px;
  color: rgb(91, 91, 91);
}

.li-day:hover {
  background-color: #717de2;
  color: #fff;
  font-size: 15px;
  cursor: pointer;
}

.li-day[isThisMonth="1"] {
  color: rgb(190, 190, 190);
  font-size: 10px;
}

.li-day[isThisMonth="1"]:hover {
  background-color: #717de2;
  color: #fff;
  font-size: 15px;
  cursor: pointer;
}

.li-day[isThisMonth="3"] {
  border-radius: 5px;
  color: rgb(255, 255, 255);
  background-color: #79bbff;
  font-weight: 600;
}
</style>