# vue-sortable-tab

> 可拖拽排序的VUE-TAB组件

## Install
```shell
npm install vue-sortable-tab -S || yarn add vue-sortable-tab
```

## Links

- [issues](https://github.com/mrKylinZhou/vue-sortable-tab/issues)
- [log](./CHANGELOG.md)

## Quick Start
```html
<template>
  <div class="s">
    <tab v-model="value" :lists="lists" @add="add" @sorted="sorted">
      <tab-head
        slot="head"
        v-for="item in lists"
        :key="item.value"
        :value="item.value">
        {{ item.value }}
      </tab-head>
      <tab-content
        v-for="item in lists"
        :key="item.value"
        :value="item.value">
        {{ item.value }}
      </tab-content>
    </tab>
  </div>
</template>

<script>
import TAB from 'vue-sortable-tab'

export default {
  name: 'HelloWorld',

  data() {
    return {
      value: '1',
      lists: [
        {
          name: '1',
          value: '1'
        },
        {
          name: '2',
          value: '2'
        },
        {
          name: '3',
          value: '3'
        },{
          name: '4',
          value: '4'
        }
      ]
    }
  },

  components: {
    Tab: TAB.Tab,
    TabHead: TAB.TabHead,
    TabContent: TAB.TabContent
  },
  
  methods: {
    add() {
      const k = (this.lists.length + 1).toString()
      this.lists.push({
        name: k,
        value: k
      })
    },
    sorted(v) {
      this.value = v.moveItem.value
    }
  }
}
</script>

```

## LICENSE
MIT
