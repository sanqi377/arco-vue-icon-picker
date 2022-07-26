<template>
  <div style="width: 500px; margin: 0 auto; margin-top: 100px">
    <icon-picker v-model:icon="icon">
      <template #iconSelect>
        <Input v-model="icon">
          <template #prefix>
            <IconApps />
          </template>
        </Input>
      </template>
    </icon-picker>
  </div>
</template>

<script lang="ts">
import { Input } from '@arco-design/web-vue';
import { IconApps } from '@arco-design/web-vue/es/icon';
// eslint-disable-next-line import/no-unresolved
import IconPicker from '@sanqi377/arco-vue-icon-picker';

// 目前vue-live导出defineComponent存在问题，所以仅支持普通对象导出。
export default {
  name: 'BasicDemo',
  components: {
    IconPicker,
    Input,
    IconApps,
  },
  data() {
    return {
      icon: '',
    };
  },
};
</script>
