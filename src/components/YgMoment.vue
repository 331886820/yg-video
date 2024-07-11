<template>
  <div id="data-view">
    <h1>{{ time }}</h1>
    <h1>{{ week }}</h1>
    <h1>{{ beforeTime }}</h1>
  </div>
</template>

<script>
import { dateFormat, getWeek, beforeDateFormat } from "yg-moment";
export default {
  name: "YgMoment",
  data() {
    return {
      time: "",
      week: "",
      beforeTime: "",
    };
  },
  created() {
    this.init();
    setInterval(() => {
      this.init();
    }, 500);
  },
  methods: {
    /**
     * getWeek此函数用于获取当前日期或指定日期是星期几，并以中文形式返回。
     * 如果未提供时间戳，则使用当前周几。
     * 返回值为0-6，分别代表星期日到星期六。
     * @param {string} [timestamp] - 可选参数，表示要查询的时间戳。如果未提供，则使用当前时间。
     * @returns {string} 中文表示的星期几，例如"星期一"。
     */
    getWeek() {
      this.week = getWeek();
    },

    /**
     * beforeDateFormat此函数用于获取指定日期离当前时间间隔多久，并以中文形式返回。（1年前，1个月前，1周前，1天前，1小时前，1分钟前，刚刚。）
     * 如果未提供时间戳，返回undefined 页面不报错也不做任何显示。
     * 返回值为0-6，分别代表星期日到星期六。
     * @param {string} [timestamp] - 必传参数，表示要查询的时间戳 必须是当前时间之前的时间戳，否则会返回 刚刚。
     * @returns {string}返回【 1年前，1个月前，1周前，1天前，1小时前，1分钟前，刚刚】
     */
    beforeDateFormat() {
      // this.beforeTime = beforeDateFormat(new Date('2024-07-04 18:30:00').getTime());
      this.beforeTime = beforeDateFormat(new Date("2024-07-04").getTime());
    },

    /**
     * getTime此函数用于获取时间  格式默认返回：YYYY-mm-dd HH:MM:SS   年-月-日 时：分：秒
     * 如果未提供时间戳，返回当前时间。
     * @param {string} [format="YYYY-mm-dd HH:MM:SS"] - 必传参数，表示要格式化的时间格式
     * @param {string} [timestamp] - 可选参数，表示要查询的时间戳，不传则默认为当前时间戳
     * @returns {string}返回  年-月-日 时：分：秒
     */
    getTime() {
      //    this.time = dateFormat("YYYY-mm-dd");// 返回 2024-07-04
      this.time = dateFormat("YYYY-mm-dd HH:MM:SS"); // 返回 2024-07-04 18:30:00
    },
	
    init() {
      this.getTime();
      this.getWeek();
      this.beforeDateFormat();
    },
  },
};
</script>

<style scoped>
</style>