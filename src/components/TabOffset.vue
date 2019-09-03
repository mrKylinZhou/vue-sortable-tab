<template>
  <div class="k-tab-offset-control">
    <span class="k-tab-offset-control-pre"
      @click="$emit('left')"
    >
      <i class="iconfont icon-cq-icon-arrow-left pre-icon" />
    </span>
    <span class="k-tab-offset-control-next"
      @click="$emit('right')"
    >
      <i class="iconfont icon-cq-icon-arrow-right next-icon" />
    </span>
  </div>
</template>

<style lang="scss">
.k-tab-offset-control {
  font-size: 12px;
  width: 42px;
  display: block;
}

.k-tab-offset-control-pre,
.k-tab-offset-control-next {
  display: inline-block;
  cursor: pointer;
}

.k-tab-offset-control .pre-icon,
.k-tab-offset-control .next-icon {
  transform: scale(0.8);
  background-color: #f9fafc;
  border: 1px solid #dde2e8;
  width: 22px;
  height: 22px;
  color: #ccc;
  text-align: center;
  line-height: 22px;
}

.k-tab-offset-control .pre-icon {
  margin-right: -1px;
}

.k-tab-offset-control .pre-icon:hover,
.k-tab-offset-control .next-icon:hover {
  background-color: #eff1f5;
}
</style>
