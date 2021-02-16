<template>
  <div id="app">
    <input
      type="text"
      @blur="handleBlur"
      @focus="handleFocus($event)"
      class="auto-input"
    />
    <ul class="dataContainer" v-show="isShow">
      <li v-for="item in data" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      isShow: false,
      data: [],
    };
  },
  methods: {
    // 聚焦事件
    handleFocus() {
      // 页面刷新后重新加载
      if(JSON.parse(localStorage.getItem('data')) && this.data.length === 0) {
        this.data = JSON.parse(localStorage.getItem('data'));
      }
      this.isShow = true;
    },
    // 失焦事件
    handleBlur(event) {
      // 判断输入是否为空
      if (event.target.value !== "") {
        // 第一次输入，将数据保存自localStoreage
        if (!JSON.parse(localStorage.getItem("data"))) {
          this.data.push(event.target.value);
          localStorage.setItem("data", JSON.stringify(this.data));
        } else {
          // 第二次输入，若localStoreage里没有存储新的数据则存下来
          let lock = JSON.parse(localStorage.getItem("data")).every((item) => {
            return item !== event.target.value;
          });
          if (lock) {
            this.data.push(event.target.value);
            localStorage.setItem("data", JSON.stringify(this.data));
          }
        }
      }
      this.isShow = false;
    },
  },
  components: {},
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
html,
body {
  width: 100%;
  height: 100%;
}
ul,
li {
  list-style: none;
}
#app {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: linear-gradient(to right bottom, red, yellow);
  /* position: relative; */
}
.auto-input {
  width: 200px;
  height: 35px;
  border: none;
  border-radius: 6px;
  outline: none;
  box-sizing: border-box;
  padding: 0 20px;
  /* background: #2c3e50; */
}
.dataContainer {
  position: absolute;
  left: 50%;
  top: 55%;
  transform: translate(-50%, 0);
  width: 500px;
  height: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: #fff;
}
</style>
