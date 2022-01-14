<template>
  <div id="app">
    <SelfAdaptionView
      :list="list"
      :minWidth="minWidth"
      :width="width">
      <template slot="source" slot-scope="{ source }">
        <p class="demo" v-for="(item, index) in source" :key="index">{{item}}</p>
      </template>
    </SelfAdaptionView>
  </div>
</template>

<script>
import SelfAdaptionView from './components/vue-self-adaption-view';

export default {
  name: 'App',
  components: {
    SelfAdaptionView,
  },
  data() {
    return {
      list: [1, 2, 3, 4, 5, 6, 7],
      minWidth: 300,
      width: 144,
      timer: null
    }
  },
  mounted() {
    this.timer = setInterval(() => {
      const listLen = this.list.length
      this.list.push(this.list[listLen - 1] + 1)
      if(listLen > 10) {
        clearInterval(this.timer)
      }
    }, 3000)
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.demo {
  width: 140px;
  height: 40px;
  line-height: 40px;
  display: inline-block;
  background: yellow
}
</style>
