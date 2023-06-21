### Vue3中实现移动端列表拖动排序
> 使用 `vue3 + ts` 技术栈，实现 `移动端【H5】` 上的列表拖动排序功能

#### 完整 demo
```vue
<template>
  <div class="dragWrapper">
    <div class="item" v-for="(item, index) in list" :key="item.id"
      :style="{ transform: `translate3d(0, ${offsets[index]}px, 0)`, zIndex: index === activeIndex ? 1 : 0}"
      @touchstart="handleTouchStart($event, item, index)"
      @touchmove="handleTouchMove($event, item, index)"
      @touchend="handleTouchEnd($event, item, index)"
      ref="itemsRef"
    >
      <van-cell-group inset>
        <van-cell :title="item.name" center>
          <template #right-icon>
            <div class="itemInfo">
              <van-image
                width="45"
                height="45"
                radius="8px"
                :src="'https://fastly.jsdelivr.net/npm/@vant/assets/cat.jpeg'"
              />
            </div>
          </template>
        </van-cell>
      </van-cell-group>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, reactive } from 'vue';

interface Item {
  id: number;
  name: string;
}

const list = reactive<Item[]>([
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' },
  { id: 3, name: 'Item 3' },
  { id: 4, name: 'Item 4' },
]);
const offsets = reactive<number[]>([]); // 记录每个元素的偏移量
const touchStartY = ref(0); // 记录触摸开始的位置
const activeIndex = ref(-1); // 记录拖拽的元素的索引
const itemsRef = ref<HTMLElement[]>([]); // 记录所有元素的ref
// 拖拽开始
const handleTouchStart = (event: TouchEvent, item: Item, index: number) => {
  // 阻止默认事件
  event.preventDefault();
  // 记录初始位置、偏移量、索引
  touchStartY.value = event.touches[0].clientY;
  activeIndex.value = index;
};

// 拖拽中
const handleTouchMove = (event: TouchEvent, item: Item, index: number) => {
  const deltaY = event.touches[0].clientY - touchStartY.value;
  offsets[index] = deltaY;
}

// 拖拽结束
const handleTouchEnd = (event: TouchEvent, item: Item, index: number) => {
  const items = Array.from(itemsRef.value);
  const itemHeight = items[0].clientHeight || 0;
  const deltaY = event.changedTouches[0].clientY - touchStartY.value;
  // 计算出当前deltaY对应的元素的索引
  const targetIndex = Math.round(deltaY / itemHeight) + activeIndex.value;
  // 为正数时，向后拖拽，为负数时，向前拖拽【更新数据】
  list.splice(targetIndex, 0, list.splice(activeIndex.value, 1)[0]);
  activeIndex.value = -1;
  // 恢复元素的位置
  items.forEach((item, index) => {
    offsets[index] = 0
  });
}
</script>
<style scoped lang="scss">
.item {
  position: relative;
  display: flex;
  align-items: center;
  .delIcon {
    position: relative;
    width: 24px;
    height: 24px;
    margin-left: 16px;
    background: #F64C4C;
    border-radius: 50%;
    &::after {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 12px;
      height: 2px;
      background: #fff;
      border-radius: 10px;
    }
  }
  &:not(:last-child) {
    margin-bottom: 8px;
  }
  .van-cell-group {
    flex: 1;
    transition: all 0.3s;
  }
  .itemInfo {
    display: flex;
    align-items: center;
    .title {
      margin-left: 8px;
      font-weight: 400;
      font-size: 16px;
      color: #252832;
    }
  }
}
</style>
```

#### 封装成组件使用
> 为了方便代码逻辑复用性，故将实现逻辑封装成组件使用
##### 组件封装
```vue
<template>
  <div class="dragWrapper">
    <div
      ref="itemsRef"
      :class="[props.class, 'drag__item']"
      v-for="(item, index) in listData" :key="item.id || index"
      :style="{ transform: `translate3d(0, ${offsets[index]}px, 0)`, zIndex: index === activeIndex ? 1 : 0}"
      @touchstart="handleTouchStart($event, item, index)"
      @touchmove="handleTouchMove($event, item, index)"
      @touchend="handleTouchEnd($event, item, index)"
    >
      <slot :item="item"></slot>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, reactive, PropType, nextTick } from 'vue';

interface IListDataItem {
  [key: string]: any;
}

const props = defineProps({
  class: {
    type: String,
    default: '',
  },
  listData: {
    type: Array as PropType<IListDataItem[]>,
    default: () => [],
  },
})
const emits = defineEmits(['update:listData'])

const offsets = reactive<number[]>([]); // 记录每个元素的偏移量
const touchStartY = ref(0); // 记录触摸开始的位置
const activeIndex = ref(-1); // 记录拖拽的元素的索引
const itemsRef = ref<HTMLElement[]>([]); // 记录所有元素的ref
// 拖拽开始
const handleTouchStart = (event: TouchEvent, item: IListDataItem, index: number) => {
  // 阻止默认事件
  event.preventDefault();
  // 记录初始位置、偏移量、索引
  touchStartY.value = event.touches[0].clientY;
  activeIndex.value = index;
};

// 拖拽中
const handleTouchMove = (event: TouchEvent, item: IListDataItem, index: number) => {
  const deltaY = event.touches[0].clientY - touchStartY.value;
  offsets[index] = deltaY;
}

// 拖拽结束
const handleTouchEnd = (event: TouchEvent, item: IListDataItem, index: number) => {
  const items = Array.from(itemsRef.value);
  const itemHeight = items[0].clientHeight || 0;
  const deltaY = event.changedTouches[0].clientY - touchStartY.value;
  // 计算出当前deltaY对应的元素的索引
  const targetIndex = Math.round(deltaY / itemHeight) + activeIndex.value;
  // 为正数时，向后拖拽，为负数时，向前拖拽【更新数据】
  props.listData.splice(targetIndex, 0, props.listData.splice(activeIndex.value, 1)[0]);
  activeIndex.value = -1;
  // 恢复元素的位置
  items.forEach((item, index) => {
    offsets[index] = 0
  })
  // 更新数据
  nextTick(() => {
    emits('update:listData', props.listData)
  })
}
</script>
<style scoped lang="scss">
.drag__item {
  position: relative;
}
</style>
```

##### 组件使用
```vue
<template>
<DragList
  :listData="listData"
  v-slot="{ item }"
  class="item"
  @update:listData="handleUpdateDragList"
>
  {{ item.name }}
</DragList>
</template>
<script lang="ts" setup>
import DragList from "@/components/drag-list.vue";
...

// 更新拖拽排序后的列表数据
const handleUpdateDragList = (list: any[]) => {
  console.log(list, 'list');
}
</script>
```

【注意⚠️】：当使用自定义 `class` 时，可能会因为组件之间的隔离性或者说内容太深，从而导致没有效果，这时候可以考虑提高 `class` 的 **权重** 以达到效果