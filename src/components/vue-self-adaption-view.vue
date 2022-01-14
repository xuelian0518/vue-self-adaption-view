<template>
  <div 
    ref="selfAdaptionView"
    class="self-adaption-view"
    :style="{
      backgroundColor: backgroundColor,
      minWidth: minWidth + 'px'
    }"
  >
    <img class="prev" :src="arrowIcon" v-show="prevVisible" @click="handlePrev" :style="{filter: `drop-shadow(${paginationColor} 2px 0)`}">
    <img class="next" :src="arrowIcon" v-show="nextVisible" @click="handleNext" :style="{filter: `drop-shadow(${paginationColor} 2px 0)`}">
    <slot name="source" :source="source"></slot>
  </div>
</template>

<script>
import arrowIcon from '../assets/icon.svg'
export default {
  name: 'SelfAdaptionView',
  props: {
    backgroundColor: {
      type: String,
      require: false,
      default: '#fff',
    },
    paginationColor: {
      type: String,
      require: false,
      default: '#000',
    },
    list: {
      type: Array,
      require: true,
    },
    width: {
      type: Number,
      require: true,
    },
    minWidth: {
      type: Number,
      require: true,
    },
  },
  data() {
    return {
      prevVisible: false,
      nextVisible: false,
      containerWidth: 0,
      source: [],
      pageSize: 0,
      listLen: 0,
      startIndex: 0,
      endIndex: 0,
      arrowIcon: arrowIcon,
    };
  },
  watch: {
    list: {
      deep: true,
      handler(arr) {
        this.updateSource()
      }
    }
  },
  mounted() {
    const resizeObserver = new ResizeObserver(entries => {
      for (const entry of entries) {
        const width = entry.contentRect.width
        this.containerWidth = width > this.minWidth ? width : this.minWidth;
        this.pageSize = (this.containerWidth / this.width) | 0
        this.updateSource()
      }
    });
    resizeObserver.observe(this.$refs.selfAdaptionView);
  },
  methods: {
    handlePrev() {
      const { startIndex, endIndex } = this
      this.nextVisible = true
      if (startIndex * 2 - endIndex > 0) {
        this.startIndex = startIndex * 2 - endIndex
        this.endIndex = startIndex
      } else {
        this.prevVisible = false
        this.startIndex = 0
        this.endIndex = endIndex - startIndex
      }
      this.source = this.list.slice(this.startIndex, this.endIndex)
    },
    handleNext() {
      const { listLen, startIndex, endIndex } = this
      this.prevVisible = true
      if (listLen - endIndex > endIndex - startIndex) {
        this.startIndex = endIndex
        this.endIndex =  endIndex * 2 - startIndex
      } else {
        this.nextVisible = false
        this.startIndex = listLen - endIndex + startIndex
        this.endIndex = listLen
      }
      this.source = this.list.slice(this.startIndex, this.endIndex)
    },
    updateSource() {
      this.listLen = this.list.length
      const { pageSize, listLen, startIndex, endIndex } = this
      if (pageSize < listLen) {
        this.nextVisible = true
        if (pageSize == endIndex - startIndex && startIndex == 0) {
          this.endIndex = pageSize
        } else {
          if (startIndex + pageSize <= listLen) {
            this.startIndex = startIndex
            this.endIndex = startIndex + pageSize
            if (startIndex + pageSize == listLen) this.nextVisible = false
            if (startIndex == 0) this.prevVisible = false
          } else {
            this.startIndex = listLen - pageSize
            this.endIndex = listLen
          }
        }
      } else {
        this.nextVisible = false
        this.startIndex = 0
        this.endIndex = listLen
        this.prevVisible = false
      }
      this.source = this.list.slice(this.startIndex, this.endIndex)
    }
  },
};
</script>

<style scoped>
  .self-adaption-view {
    position: relative;
  }
  .prev, .next {
    width: 16px;
    height: 16px;
    position: absolute;
    top: 50%;
    margin-top: -8px;
    cursor: pointer;
  }
  .prev {
    left: 10px;
  }
  .next {
    right: 10px;
    transform: rotate(180deg);
  }
</style>
