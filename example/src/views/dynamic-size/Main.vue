<template>
  <div class="example">
    <github-corner />
    <introduction description="The size of each item is dynamic, you don't have to care about size, it will calculate automatic, but you have to make sure that there's an unique id for every array data." />

    <div class="example-content">
      <tab v-on:tab-change="onTabChange" />

      <div v-show="isShowView">
        <virtual-list class="list-dynamic scroll-touch"
          :size="80"
          :keeps="30"
          :item-class="'list-item-dynamic'"

          :data-key="'id'"
          :data-sources="items"
          :data-component="itemComponent"
        />
      </div>

      <codeblock v-show="!isShowView" />
    </div>
  </div>
</template>

<script>
import Item from './Item'
import Code from './Code'

import { isMobile } from '../../common/ua'
import { Random } from '../../common/mock'
import genUniqueId from '../../common/gen-unique-id'
import { TOTAL_COUNT, TAB_TYPE, DEFAULT_TAB } from '../../common/const'

const DataItems = []
let count = TOTAL_COUNT
while (count--) {
  const index = TOTAL_COUNT - count
  DataItems.push({
    index,
    name: Random.name(),
    id: genUniqueId(index),
    desc: Random.paragraph(Random.integer(0, isMobile ? 1 : 3))
  })
}

export default {
  name: 'dynamic-size',

  components: {
    codeblock: Code
  },

  data () {
    return {
      items: DataItems,
      itemComponent: Item,
      isShowView: DEFAULT_TAB === TAB_TYPE.VIEW
    }
  },

  methods: {
    onTabChange (type) {
      this.isShowView = type === TAB_TYPE.VIEW
    }
  }
}
</script>

<style lang="less">
.list-dynamic {
  width: 100%;
  height: 500px;
  border: 2px solid;
  border-radius: 3px;
  overflow-y: auto;
  border-color: dimgray;

  .list-item-dynamic {
    display: flex;
    align-items: center;
    padding: 1em;
    border-bottom: 1px solid;
    border-color: lightgray;
  }
}
</style>
