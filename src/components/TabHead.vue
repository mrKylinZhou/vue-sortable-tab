<template>
  <div
    @click="handleClick"
    class="k-tab-head-item"
    :class="{
      active: active
    }"
  >
    <label class="k-tab-head-label">
      <slot></slot>
    </label>
    <span
      class="k-tab-head-before"
      :class="{
        active: active
      }"
    >
    </span>
    <span
      class="k-tab-head-after"
      :class="{
        active: active
      }"
    >
    </span>
    <i
      v-if="closable"
      class="iconfont icon-tab_close"
      @click.stop="handleRemove">
    </i>
  </div>
</template>

<script lang="ts">
import isFunction from 'lodash/isFunction'

export default {
  props: {
    value: {
      type: [String, Number],
      required: true
    },
    closable: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    active() {
      return this.$parent.currentValue === this.value
    }
  },

  methods: {
    handleClick() {
      if (isFunction(this.$parent.tabClick)) {
        this.$parent.tabClick(this.value)
      }
    },

    handleRemove() {
      this.$emit('close')
    }
  }
}
</script>

<style lang="scss">
.k-tab-head-item {
  cursor: pointer;
  border-top: 1px solid #D2D9E4;
  display: inline-block;
  position: relative;
  margin: 0 15px;
  width: 150px;
  height: 30px;
  font-size: 14px;
  line-height: 30px;
  background-color: #E4E9F3;
  color: #666;

  .icon-tab_close {
    z-index: 10;
    position: absolute;
    right: -7px;
    top: 4px;
    width: 20px;
    height: 20px;
    line-height: 20px;
    text-align: center;
    font-size: 16px;
    color: #A9B1C5;

    &:hover {
      cursor: pointer;
      &::before {
        content: "\e7e6";
      }
    }
  }

  &.active {
    background-color: #FFFFFF;
    border-bottom: 1px solid transparent;
    width: auto;
    min-width: 150px;
    max-width: 300px;
  }

  &:first-child {
    .k-tab-head-before {
      transform: skewX(0);
    }
  }

  .k-tab-head-before, .k-tab-head-after {
    border-top: 1px solid #D2D9E4;
    top: -1px;
    position: absolute;
    width: 25px;
    height: 30px;
    background-color: #E4E9F3;

    &.active {
      border-top: 1px solid #DDE2E8;
      border-bottom: 1px solid transparent;
      background-color: #FFFFFF;
      z-index: 8;
    }
  }

  .k-tab-head-before {
    border-left: 1px solid #D2D9E4;
    transform: skewX(-20deg);
    left: -15px;
  }

  .k-tab-head-after {
    border-right: 1px solid #D2D9E4;
    transform: skewX(20deg);
    right: -15px;
  }

  .k-tab-head-label {
    position: relative;
    z-index: 10;
    left: 0;
    top: 0;
  }
}
</style>
