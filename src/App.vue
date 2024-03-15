<script setup lang="ts">
import { computed } from 'vue'
import { useAppStore } from '@/store/modules/app'
import { ConfigGlobal } from '@/components/ConfigGlobal'
// import { useDesign } from '@/hooks/web/useDesign'
// import { ElNotification } from 'element-plus'

// const { getPrefixCls } = useDesign()

// const prefixCls = getPrefixCls('app')

const appStore = useAppStore()

const currentSize = computed(() => appStore.getCurrentSize)

// ElNotification({
//   title: '팁',
//   type: 'warning',
//   duration: 0,
//   dangerouslyUseHTMLString: true,
//   message:
//     '<div><p><strong>결정이 어려울 때는 먼저 자주 묻는 질문을 참조하세요. 아마도 당신이 필요한 관련 답변을 찾을 수 있습니다.</strong></p><p><a href="https://element-plus-admin-doc.cn/guide/fqa.html" target="_blank">링크(중국어)</a></p></div>'
// })
</script>

<template>
  <ConfigGlobal :size="currentSize">
    <RouterView />
  </ConfigGlobal>
</template>

<style lang="less">
@prefix-cls: ~'@{namespace}-app';

.size {
  width: 100%;
  height: 100%;
}

html,
body {
  padding: 0 !important;
  margin: 0;
  overflow: hidden;
  .size;

  #app {
    .size;
  }
}

.@{prefix-cls}-grey-mode {
  filter: grayscale(100%);
}
</style>
