<template>
  <div class="k-tab-add-btn"
    @click.stop="$emit('add')"
  >
  </div>
</template>

<style lang="scss" scoped>
.k-tab-add-btn {
  cursor: pointer;
  display: block;
  border-radius: 3px;
  border: 1px solid #DDE2E8;
  background-color: #FFFFFF;
  width: 15px;
  height: 20px;
  transform: skewX(20deg);
  box-sizing: border-box;
  z-index: 20;
}
</style>
