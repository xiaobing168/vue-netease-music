<template>
  <div
    class="progress-bar"
    ref="progressBar"
    @click="progressClick"
  >
    <div class="bar-inner">
      <div
        class="progress"
        ref="progress"
      ></div>
      <div
        class="progress-btn-wrapper"
        ref="progressBtn"
      >
        <div
          class="progress-btn"
          :class="{show: alwaysShowBtn}"
        ></div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import { prefixStyle } from "@/utils/dom"

const transform = prefixStyle("transform")

export default {
  name: 'ProgressBar',
  props: {
    percent: {
      type: Number,
      default: 0
    },
    alwaysShowBtn: {
      type: Boolean,
      default: false
    }
  },
  created() {
    this.touch = {}
  },
  mounted() {
    if (this.percent > 0) {
      this.setProgressOffset(this.percent)
    }
  },
  methods: {
    progressClick(e) {
      const rect = this.$refs.progressBar.getBoundingClientRect()
      const offsetWidth = Math.max(0, Math.min(e.pageX - rect.left, this.$refs.progressBar.clientWidth))
      this._offset(offsetWidth)
      // 这里当我们点击 progressBtn 的时候，e.offsetX 获取不对
      // this._offset(e.offsetX)
      this._triggerPercent()
    },
    setProgressOffset(percent) {
      if (percent >= 0 && !this.touch.initiated) {
        const barWidth = this.$refs.progressBar.clientWidth
        const offsetWidth = percent * barWidth
        this._offset(offsetWidth)
      }
    },
    _triggerPercent() {
      this.$emit("percentChange", this._getPercent())
    },
    _offset(offsetWidth) {
      this.$refs.progress.style.width = `${offsetWidth}px`
      this.$refs.progressBtn.style[
        transform
      ] = `translate3d(${offsetWidth}px,0,0)`
    },
    _getPercent() {
      const barWidth = this.$refs.progressBar.clientWidth
      return this.$refs.progress.clientWidth / barWidth
    }
  },
  watch: {
    percent(newPercent) {
      this.setProgressOffset(newPercent)
    }
  }
}
</script>

<style scoped lang="scss">
.progress-bar {
  height: 30px;
  cursor: pointer;

  .bar-inner {
    position: relative;
    top: 14px;
    height: 2px;
    background: var(--progress-bgcolor);

    .progress {
      position: absolute;
      height: 100%;
      background: $theme-color;
    }

    .progress-btn-wrapper {
      position: absolute;
      left: -15px;
      top: -13px;
      width: 30px;
      height: 30px;

      .progress-btn {
        display: none;
        position: relative;
        top: 8px;
        left: 9px;
        box-sizing: border-box;
        width: 12px;
        height: 12px;
        border-radius: 50%;
        background: $theme-color;

        &.show {
          display: block;
        }
      }
    }
  }

  &:hover {
    .progress-btn {
      display: block !important;
    }
  }
}
</style>
