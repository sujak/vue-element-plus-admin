<script setup lang="ts">
import { ElSwitch } from 'element-plus'
import { useI18n } from '@/hooks/web/useI18n'
import { useAppStore } from '@/store/modules/app'
import { computed, ref, watch } from 'vue'

const appStore = useAppStore()

const { t } = useI18n()

// 面包屑
const breadcrumb = ref(appStore.getBreadcrumb)

const breadcrumbChange = (show: boolean) => {
  appStore.setBreadcrumb(show)
}

// 面包屑图标
const breadcrumbIcon = ref(appStore.getBreadcrumbIcon)

const breadcrumbIconChange = (show: boolean) => {
  appStore.setBreadcrumbIcon(show)
}

// 折叠图标
const hamburger = ref(appStore.getHamburger)

const hamburgerChange = (show: boolean) => {
  appStore.setHamburger(show)
}

// 全屏图标
const screenfull = ref(appStore.getScreenfull)

const screenfullChange = (show: boolean) => {
  appStore.setScreenfull(show)
}

// 尺寸图标
const size = ref(appStore.getSize)

const sizeChange = (show: boolean) => {
  appStore.setSize(show)
}

// 多语言图标
const locale = ref(appStore.getLocale)

const localeChange = (show: boolean) => {
  appStore.setLocale(show)
}

// 标签页
const tagsView = ref(appStore.getTagsView)

const tagsViewChange = (show: boolean) => {
  appStore.setTagsView(show)
}

// logo
const logo = ref(appStore.getLogo)

const logoChange = (show: boolean) => {
  appStore.setLogo(show)
}

// 固定头部
const fixedHeader = ref(appStore.getFixedHeader)

const fixedHeaderChange = (show: boolean) => {
  appStore.setFixedHeader(show)
}

// 页脚
const footer = ref(appStore.getFooter)

const footerChange = (show: boolean) => {
  appStore.setFooter(show)
}

// 灰色模式
const greyMode = ref(appStore.getGreyMode)

const greyModeChange = (show: boolean) => {
  appStore.setGreyMode(show)
}

const layout = computed(() => appStore.getLayout)

watch(
  () => layout.value,
  (n) => {
    if (n === 'top') {
      appStore.setCollapse(false)
    }
  }
)
</script>

<template>
  <div class="v-interface-display">
    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.breadcrumb') }}</span>
      <ElSwitch v-model="breadcrumb" @change="breadcrumbChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.breadcrumbIcon') }}</span>
      <ElSwitch v-model="breadcrumbIcon" @change="breadcrumbIconChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.hamburgerIcon') }}</span>
      <ElSwitch v-model="hamburger" @change="hamburgerChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.screenfullIcon') }}</span>
      <ElSwitch v-model="screenfull" @change="screenfullChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.sizeIcon') }}</span>
      <ElSwitch v-model="size" @change="sizeChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.localeIcon') }}</span>
      <ElSwitch v-model="locale" @change="localeChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.tagsView') }}</span>
      <ElSwitch v-model="tagsView" @change="tagsViewChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.logo') }}</span>
      <ElSwitch v-model="logo" @change="logoChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.fixedHeader') }}</span>
      <ElSwitch v-model="fixedHeader" @change="fixedHeaderChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.footer') }}</span>
      <ElSwitch v-model="footer" @change="footerChange" />
    </div>

    <div class="flex justify-between items-center">
      <span class="text-14px">{{ t('setting.greyMode') }}</span>
      <ElSwitch v-model="greyMode" @change="greyModeChange" />
    </div>
  </div>
</template>