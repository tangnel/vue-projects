<template>
    <div class="calender">
        <div class="date-header">
            <div class="prev-month" @click="handlePrev"></div>
            <div class="show-date">{{year}}年{{month}}月{{day}}日</div>
            <div class="next-month" @click="handleNext"></div>
        </div>
        <div class="date-content">
            <div class="week-header">
                <div 
                    v-for="item in ['日','一','二','三','四','五','六']"
                    :key="item"
                >
                {{item}}
                </div>
            </div>
            <div class="week-day">
                <div class="every-day"
                    v-for="item in 42"
                    :key="item">
                    
                    <div 
                    v-if="item - beginDay >0 && item -beginDay <= curDays"
                    :class="{'now-day':`${year}-${month}-${item - beginDay}`===curDate,
                    'active-day':`${year}-${month}-${item - beginDay}`=== `${year}-${month}-${day}`}"
                    :data-day="item - beginDay"
                    @click="handleChooseDay">
                    {{item - beginDay}}
                    </div>
                    <div class="other-day" v-else-if="item -beginDay <=0">{{item - beginDay + prevDays }}</div>
                    <div class="other-day" v-else>{{item - curDays - beginDay}}</div>
                </div>
            </div>
        </div>
    </div>

</template>
<script>
export default {
    data(){
        return {
            year:null,
            month:null,
            day:null,
            curDate:null
        }
    },
    created(){
        this.getInitTime();
        //new Date(this.year, this.month -1,1) 获取这个月的第一天
    },
    methods:{
        getInitTime(){
            const date= new Date();
            this.year = date.getFullYear();
            this.month = date.getMonth() +1;
            this.day = date.getDate();
            this.curDate = `${this.year}-${this.month}-${this.day}`
        },
        handleChooseDay(e){
          this.day = e.target.dataset.day;
        },
        handlePrev(){
          if(this.month == 1){
            this.year--;
            this.month = 12;
          }else{
            this.month--;
          }
          this.computedDay();
        },
        handleNext(){
          if(this.month == 12){
            this.year++;
            this.month = 1;
          }else{
            this.month++;
          }
          this.computedDay();
        },
        computedDay(){
          const allDay = new Date(this.year, this.month,0).getDate();
          if(this.day > allDay){
            this.day = allDay;
          }
        }
    },
    computed:{
        beginDay(){
            //获取这个月的第一天是星期几
            //console.log(new Date(this.year, this.month -1,1).getDay())
           return new Date(this.year, this.month -1,1).getDay();
        },
        curDays(){
            //获取这个月的总天数
           return new Date(this.year, this.month, 0).getDate();
        },
        prevDays(){
            //获取上个月的总天数
            return new Date(this.year, this.month -1, 0).getDate();
        }
    }


}
</script>
<style scoped>
 .calender {
    width: 500px;
    margin-left: 100px;
  }

  .date-header {
    width: 100%;
    display: flex;
    line-height: 30px;
  }

  .prev-month,
  .next-month {
    border: 15px solid transparent;
    width: 0;
    height: 0;
    cursor: pointer;
  }

  .prev-month {
    border-right-color: #007fff;
  }

  .next-month {
    border-left-color: #007fff;
  }

  .show-date {
    flex: 1;
    text-align: center;
    color: #007fff;
  }
  .week-header {
    display: flex;
  }

  .week-header > div {
    flex: 1;
    text-align: center;
    line-height: 30px;
    background-color: #007fff;
    color: #fff;
    font-weight: 600;
  }

  .week-day {
    width: 100%;
  }
  .every-day {
    
    display: inline-block;
    width: 14.28%;
    line-height: 50px;
    text-align: center;
    cursor: pointer;
  }
  .other-day {
    color: #ccc;
  }
  .now-day {
    background-color: #007fff;
    color: #fff;
    font-weight: 600;
  }
  .active-day:not(.now-day) {
    color: #007fff;
    border: 2px solid #007fff;
    line-height: 46px;
  }
</style>