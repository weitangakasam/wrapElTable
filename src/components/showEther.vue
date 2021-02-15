<!--  -->
<template>
  <div>
    <div @scroll="scrollHandler" style="height: 240px; overflow: scroll">
      <div :style="{ margin: margin }">
        <div
          v-for="ele in showEhters"
          :key="ele"
          :style="{ height: `${height}px` }"
        >
          {{ ele }}
        </div>
        <div
          v-get="getMore"
          style="width: 10px; background-color: red; height: 5px"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    ethers: {
      default: [],
    },
  },
  directives: {
    get: {
      bind(el, ref) {
        let ob = new IntersectionObserver((entries) => {
          entries.forEach((entry) => {
            if (entry.target === el) {
              ref.value();
            }
          });
        });
        ob.observe(el);
      },
    },
  },
  data() {
    return {
      height: 70,
      start: 0,
      end: 4,
      scroll: "0px",
    };
  },
  computed: {
    showEhters() {
      return this.ethers.slice(this.start, this.end);
    },
    margin() {
      let res = `${this.scroll}px 0 ${
        (this.showEhters.length - this.end) * this.height
      }px 0`;
      console.log(res);
      return res;
    },
  },
  methods: {
    getMore() {
      let res = [];
      for (let i = 0; i < 10; i++) {
        res.push("vlan" + Math.random(1000));
      }
      this.ethers.splice(this.ethers.length, 0, ...res);
    },
    scrollHandler(el) {
      this.scroll = el.target.scrollTop;
      this.start = Math.ceil(this.scroll / this.height);
      this.end = Math.max(this.end, this.start + 4);
      console.log(this.start, this.end);
    },
  },
  //生命周期 - 挂载完成（访问DOM元素）
  mounted() {},
};
</script>
<style scoped>
/* @import url(); 引入css类 */
</style>
