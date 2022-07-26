<template>
  <Popover
    v-model:popup-visible="visible"
    position="bottom"
    trigger="click"
    :content-style="{ width: '450px' }"
  >
    <template #title>
      <Input
        v-model="searchValue"
        placeholder="请输入需要搜索的图标代码 ..."
        :style="{ 'margin-bottom': '8px' }"
        @input="filterIcon"
      >
        <template #suffix>
          <component :is="iconArrs['IconSearch']" />
        </template>
      </Input>
    </template>
    <template #content>
      <Row :gutter="[8, 8]">
        <Col v-for="(item, index) in iconArr" :key="index" :span="4">
          <div class="icons" @click="handleClick(item)">
            <component :is="iconArrs[item]" />
          </div>
        </Col>
      </Row>
    </template>
    <slot name="iconSelect"></slot>
  </Popover>
</template>

<script lang="ts">
import { defineComponent, ref, createApp, watch } from 'vue';
import { Popover, Input, Row, Col } from '@arco-design/web-vue';
import * as arcoIcons from '@arco-design/web-vue/es/icon';

const app = createApp(defineComponent);

const icons: any = arcoIcons;

const Arr = [
  'IconCode',
  'IconPlus',
  'IconArrowLeft',
  'IconArrowRight',
  'IconArrowRise',
  'IconArrowFall',
  'IconLeft',
  'IconRight',
];

Object.keys(icons).forEach((key) => {
  app.component(key, icons[key]);
});

export default defineComponent({
  name: 'IconPicker',
  components: {
    Popover,
    Input,
    Row,
    Col,
  },
  props: {
    /**
     * 图标代码
     */
    icon: String,
  },
  emits: ['update:icon'],
  setup(props: any, context: any) {
    const iconArr = ref<string[]>(Arr);
    const visible = ref<boolean>(false);
    const searchValue = ref('');
    const iconArrs = icons;

    const handleClick = (icon: string) => {
      visible.value = false;
      context.emit('update:icon', icon);
    };

    /**
     * 进行搜索过滤
     */
    const filterIcon = () => {
      if (searchValue.value) {
        iconArr.value = Arr.filter((item) => {
          return item.toLowerCase().includes(searchValue.value);
        });
      } else {
        iconArr.value = Arr;
      }
    };

    watch(visible, () => {
      searchValue.value = '';
      iconArr.value = Arr;
    });

    return {
      iconArrs,
      iconArr,
      handleClick,
      visible,
      searchValue,
      filterIcon,
    };
  },
});
</script>

<style scoped>
.icons {
  display: flex;
  height: 40px;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  border-radius: 4px;
  border: 1px solid #dcdfe6;
  cursor: pointer;
}

.icons:hover {
  background: var(--color-neutral-3);
}
</style>
