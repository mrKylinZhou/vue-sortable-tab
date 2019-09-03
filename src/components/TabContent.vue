<template>
  <div
    v-show="active"
    class="k-tab-content"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: [String, Number],
      required: true
    }
  },
  computed: {
    active() {
      return this.$parent.currentValue === this.value
    }
  }
}
</script>

<style lang="scss">
.k-tab-content {
  width: 100%;
  height: 100%;
}
</style>
