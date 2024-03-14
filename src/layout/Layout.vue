<script lang="tsx">
import { computed, defineComponent } from 'vue'
import { useAppStore } from '@/store/modules/app'

import { Menu } from '@/components/Menu'
import { TagsView } from '@/components/TagsView'
import { Logo } from '@/components/Logo'
import AppView from './components/AppView.vue'
import ToolHeader from './components/ToolHeader.vue'
import { ElScrollbar } from 'element-plus'

import { Backtop } from '@/components/Backtop'
import { Setting } from '@/components/Setting'
import { useDesign } from '@/hooks/web/useDesign'

const { getPrefixCls } = useDesign()

const prefixCls = getPrefixCls('layout')

const appStore = useAppStore()
const pageLoading = computed(() => appStore.getPageLoading)
const tagsView = computed(() => appStore.getTagsView)
// 是否是移动端
const mobile = computed(() => appStore.getMobile)

// 菜单折叠
const collapse = computed(() => appStore.getCollapse)

const handleClickOutside = () => {
  appStore.setCollapse(true)
}

export default defineComponent({
  name: 'Layout',
  setup() {
    return () => (
      <section class={[prefixCls, `${prefixCls}__classic`, 'w-[100%] h-[100%] relative']}>
        {mobile.value && !collapse.value ? (
          <div
            class="absolute top-0 left-0 w-full h-full opacity-30 z-99 bg-[var(--el-color-black)]"
            onClick={handleClickOutside}
          ></div>
        ) : undefined}

        <>
          <div
            class={[
              'absolute top-0 left-0 h-full layout-border__right',
              { '!fixed z-3000': mobile.value }
            ]}
          >
            <Logo
              class={[
                'bg-[var(--left-menu-bg-color)] relative',
                {
                  '!pl-0': mobile.value && collapse.value,
                  'w-[var(--left-menu-min-width)]': appStore.getCollapse,
                  'w-[var(--left-menu-max-width)]': !appStore.getCollapse
                }
              ]}
              style="transition: all var(--transition-time-02);"
            ></Logo>
            <Menu class={['!h-[calc(100%-var(--logo-height))]']}></Menu>
          </div>
          <div
            class={[
              `${prefixCls}-content`,
              'absolute top-0 h-[100%]',
              {
                'w-[calc(100%-var(--left-menu-min-width))] left-[var(--left-menu-min-width)]':
                  collapse.value && !mobile.value && !mobile.value,
                'w-[calc(100%-var(--left-menu-max-width))] left-[var(--left-menu-max-width)]':
                  !collapse.value && !mobile.value && !mobile.value,
                'fixed !w-full !left-0': mobile.value
              }
            ]}
            style="transition: all var(--transition-time-02);"
          >
            <ElScrollbar
              v-loading={pageLoading.value}
              class={[
                `${prefixCls}-content-scrollbar`,
                '!h-[calc(100%-var(--top-tool-height)-var(--tags-view-height))] mt-[calc(var(--top-tool-height)+var(--tags-view-height))]'
              ]}
            >
              <div
                class={[
                  'fixed top-0 left-0 z-10',
                  {
                    'w-[calc(100%-var(--left-menu-min-width))] !left-[var(--left-menu-min-width)]':
                      collapse.value && !mobile.value,
                    'w-[calc(100%-var(--left-menu-max-width))] !left-[var(--left-menu-max-width)]':
                      !collapse.value && !mobile.value,
                    '!w-full !left-0': mobile.value
                  }
                ]}
                style="transition: all var(--transition-time-02);"
              >
                <ToolHeader
                  class={[
                    'bg-[var(--top-header-bg-color)]',
                    {
                      'layout-border__bottom': !tagsView.value
                    }
                  ]}
                ></ToolHeader>

                {tagsView.value ? (
                  <TagsView class="layout-border__bottom layout-border__top"></TagsView>
                ) : undefined}
              </div>

              <AppView></AppView>
            </ElScrollbar>
          </div>
        </>

        <Backtop></Backtop>

        <Setting></Setting>
      </section>
    )
  }
})
</script>

<style lang="less" scoped>
@prefix-cls: ~'@{namespace}-layout';

.@{prefix-cls} {
  background-color: var(--app-content-bg-color);
  .@{prefix-cls}-content-scrollbar {
    & > :deep(.el-scrollbar__wrap) {
      & > .@{elNamespace}-scrollbar__view {
        display: flex;
        height: 100% !important;
        flex-direction: column;
      }
    }
  }
}
</style>
