<template>
  <div class="k-tab">
    <div
      class="k-tab-head"
      :class="{ 'is-scroll': isScroll }" 
    >
      <div class="k-tab-head-scroll">
        <div
          class="k-tab-head-item-wrap"
          ref="sortWrap" 
        >
          <slot name="head"></slot>
        </div>
      </div>
      <div class="k-tab-head-operate">
        <tab-add
          @add="$emit('add')"
          v-show="addable"
        />
        <tab-offset
          v-show="isScroll"
          @left="scrollToLeft"
          @right="scrollToRight"
        />
      </div>
    </div>
    <div class="k-tab-pane-wrap">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import { Sortable } from '@shopify/draggable'
import cloneDeep from 'lodash/cloneDeep'

import TabAdd from './TabAdd'
import TabOffset from './TabOffset'

export default {
  props: {
    value: {
      type: String,
      default: ''
    },
    lists: {
      type: Array,
      required: true
    },
    addable: {
      type: Boolean,
      default: false
    },
    closable: {
      type: Boolean,
      default: false
    },
    isSortabled: {
      type: Boolean,
      default: false
    }
  },

  data() {
    return {
      currentValue: this.value,
      sortable: null,
      isScroll: false
    }
  },

  watch: {
    value(value) {
      this.setCurrentValue(value)
    },
    isSortabled() {
      this.destroySort()
      this.initSort()
    }
  },

  components: {
    TabAdd,
    TabOffset
  },

  methods: {
    initListenScroll() {
      const $dom = this.$refs.sortWrap
      if (!$dom) return
      new ResizeObserver(() => {
        const { scrollWidth, clientWidth } = $dom
        this.isScroll = scrollWidth - 6 > clientWidth
      }).observe($dom)
    },

    scrollToLeft() {
      const $dom = this.$refs.sortWrap
      if (!$dom) return
      const { clientWidth, scrollLeft } = $dom
      const newOffset = clientWidth > scrollLeft
          ? 0
          : scrollLeft - clientWidth + 75
      this.setScroll(newOffset)
    },
    scrollToRight() {
      const $dom = this.$refs.sortWrap
      if (!$dom) return
      const { clientWidth, scrollLeft, scrollWidth } = $dom
      const newOffset = clientWidth * 2 + scrollLeft > scrollWidth
          ? scrollWidth
          : clientWidth + scrollLeft - 75;
      this.setScroll(newOffset)
    },

    setScroll(offset) {
      const $dom = this.$refs.sortWrap
      if (!$dom) return
      $dom.scrollLeft = offset;
    },

    destroySort() {
      if (this.sortable) {
        this.sortable.destroy()
        this.sortable = null
      }
    },

    initSort() {
      if (!this.isSortabled) return
      const $dom = this.$refs.sortWrap
      if (!$dom) return
      this.sortable = new Sortable($dom, {
        draggable: '.k-tab-head-item',
        delay: 200
      })
      this.sortable.on('sortable:start', e => {
        e.data.dragEvent.data.mirror.style.zIndex = 200
        e.data.dragEvent.data.source.style.visibility = 'hidden'
      })
      this.sortable.on('sortable:stop', e => {
        const oldIndex = e.data.oldIndex
        const newIndex = e.data.newIndex
        if (oldIndex === newIndex) return
        const moveItem = this.lists[oldIndex]
        const cacheLists = cloneDeep(this.lists)
        const spliceItem = cacheLists.splice(oldIndex, 1)[0]
        cacheLists.splice(newIndex, 0, spliceItem)
        this.$emit('sorted', {
          moveItem: moveItem,
          lists: cacheLists
        })
      })
    },

    setCurrentValue(value) {
      this.$emit('input', value)
      this.currentValue = value
    },

    tabClick(value) {
      this.setCurrentValue(value)
    }
  },

  mounted() {
    this.initListenScroll()
    this.initSort()
  },

  destroyed() {
    this.destroySort()
  }
}
</script>

<style lang="scss">
.k-tab{
  height: 100%;

  .k-tab-head {
    display: flex;
    align-items: center;

    &.is-scroll {
      justify-content: space-between;
    }
  }
  
  .k-tab-head-item-wrap {
    position: relative;
    overflow-x: hidden;
    overflow-y: hidden;
    white-space: nowrap;
    padding-right: 6px;
  }

  .k-tab-head-scroll {
    max-width: calc(100% - 80px);
    margin-right: -4px;
    overflow: hidden;
  }

  .k-tab-head-operate {
    width: 66px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-left: 4px;
  }
}
</style>
