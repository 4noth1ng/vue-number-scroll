<template>
  <div class="wrapper">
    <div class="container" ref="container" v-if="reloop">
      <div
        class="item"
        v-for="(item, index) in targetArr"
        :key="index"
        ref="target"
      >
        0123456789
      </div>
    </div>

    <button class="btn" @click="reLoop">重新加載</button>
  </div>
</template>

<script>
export default {
  name: "numberScroll",

  props: {
    // 目标数字
    target: {
      type: String,
      required: true,
    },
    // 容器宽度
    width: {
      type: Number,
      required: true,
    },
    // 容器高度
    height: {
      type: Number,
      required: true,
    },
    // 容器字体大小 建议与height一致 否则样式会乱
    fontSize: {
      type: Number,
      required: true,
    },
  },

  data() {
    return {
      targetArr: [],
      timer: null,
      reloop: true,
    };
  },

  methods: {
    numAnimation() {
      for (const str of this.target) {
        this.targetArr.push(parseInt(str));
      }
      this.$nextTick(() => {
        // 修改容器样式
        const { container } = this.$refs;
        container.style.width = `${this.width}px`;
        container.style.height = `${this.height}px`;
        container.style.fontSize = `${this.fontSize}px`;
        const { target } = this.$refs;
        const size = this.targetArr.length;
        // 修改 数字 translateY 以及其 样式
        this.targetArr.forEach((targetNum, index) => {
          const width = (100 / size).toFixed(6);
          const dom = target[index];
          dom.style.width = `${width}%`;
          dom.style.height = `${this.height * 10}px`;
          dom.style.lineHeight = `${(width / 100) * this.width}px`;
          dom.style.transition = `all linear ${0.5 + index * 0.2}s`;
          for (let i = 0; i <= targetNum; i++) {
            const idx = -i * 10;
            this.timer = setTimeout(() => {
              dom.style.transform = `translateY(${idx}%)`;
            }, 0);
          }
        });
      });
    },

    reLoop() {
      // 重新加载组件
      this.reloop = false;
      this.$nextTick(() => {
        this.reloop = true;
        this.targetArr = [];
        this.numAnimation();
      });
    },
  },

  mounted() {
    this.numAnimation();
  },

  unmounted() {
    if (this.timer) {
      clearTimeout(this.timer);
    }
  },
};
</script>

<style scoped lang="scss">
.wrapper {
  width: 1200px;

  height: 1000px;

  margin: 0 auto;

  .container {
    margin: 200px;

    font-size: 38px;

    font-weight: 500;

    display: flex;

    width: 130px;

    height: 38px;

    overflow: hidden;

    .item {
      height: 380px;

      text-orientation: upright;

      writing-mode: vertical-lr;

      transition: all linear 1s;

      text-align: center;

      &:hover {
        cursor: default;
      }
    }
  }

  .btn {
    width: 230px;

    height: 30px;

    font-size: 20px;
  }
}
</style>
