<script setup lang="ts">
import { ContentWrap } from '@/components/ContentWrap'
import { ref, unref } from 'vue'
import { ElDivider, ElImage, ElTag, ElTabPane, ElTabs, ElButton, ElMessage } from 'element-plus'
import defaultAvatar from '@/assets/imgs/avatar.jpg'
import UploadAvatar from './components/UploadAvatar.vue'
import { Dialog } from '@/components/Dialog'
import EditInfo from './components/EditInfo.vue'
import EditPassword from './components/EditPassword.vue'

const userInfo = ref()
const fetchDetailUserApi = async () => {
  // 这里可以调用接口获取用户信息
  const data = {
    id: 1,
    username: 'admin',
    realName: 'admin',
    phoneNumber: '18888888888',
    email: '502431556@qq.com',
    avatarUrl: '',
    roleList: ['超级管理员']
  }
  userInfo.value = data
}
fetchDetailUserApi()

const activeName = ref('first')

const dialogVisible = ref(false)

const uploadAvatarRef = ref<ComponentRef<typeof UploadAvatar>>()
const avatarLoading = ref(false)
const saveAvatar = async () => {
  try {
    avatarLoading.value = true
    const base64 = unref(uploadAvatarRef)?.getBase64()
    console.log(base64)
    // 这里可以调用修改头像接口
    fetchDetailUserApi()
    ElMessage.success('修改成功')
    dialogVisible.value = false
  } catch (error) {
    console.log(error)
  } finally {
    avatarLoading.value = false
  }
}
</script>

<template>
  <div class="grid w-100% lg:h-100% grid-cols-1 lg:grid-cols-2 gap-sm">
    <ContentWrap title="내 정보 관리">
      <div class="flex justify-center items-center">
        <div
          class="avatar w-[150px] h-[150px] relative cursor-pointer"
          @click="dialogVisible = true"
        >
          <ElImage
            class="w-[150px] h-[150px] rounded-full"
            :src="userInfo?.avatarUrl || defaultAvatar"
            fit="fill"
          />
        </div>
      </div>
      <ElDivider />
      <div class="flex justify-between items-center">
        <div>계정：</div>
        <div>{{ userInfo?.username }}</div>
      </div>
      <ElDivider />
      <div class="flex justify-between items-center">
        <div>이름：</div>
        <div>{{ userInfo?.realName }}</div>
      </div>
      <ElDivider />
      <div class="flex justify-between items-center">
        <div>휴대전화</div>
        <div>{{ userInfo?.phoneNumber ?? '-' }}</div>
      </div>
      <ElDivider />
      <div class="flex justify-between items-center">
        <div>이메일</div>
        <div>{{ userInfo?.email ?? '-' }}</div>
      </div>
      <ElDivider />
      <div class="flex justify-between items-center">
        <div>소속 역할：</div>
        <div>
          <template v-if="userInfo?.roleList?.length">
            <ElTag v-for="item in userInfo?.roleList || []" :key="item" class="ml-2 mb-w"
              >{{ item }}
            </ElTag>
          </template>
          <template v-else>-</template>
        </div>
      </div>
      <ElDivider />
    </ContentWrap>
    <ContentWrap title="기본 자료" class="">
      <ElTabs v-model="activeName">
        <ElTabPane label="기본 정보" name="first">
          <EditInfo :user-info="userInfo" />
        </ElTabPane>
        <ElTabPane label="비밀번호 변경" name="second">
          <EditPassword />
        </ElTabPane>
      </ElTabs>
    </ContentWrap>
  </div>

  <Dialog v-model="dialogVisible" title="프로필 사진 수정" width="800px">
    <UploadAvatar ref="uploadAvatarRef" :url="userInfo?.avatarUrl || defaultAvatar" />

    <template #footer>
      <ElButton type="primary" :loading="avatarLoading" @click="saveAvatar"> 저장 </ElButton>
      <ElButton @click="dialogVisible = false">취소</ElButton>
    </template>
  </Dialog>
</template>

<style lang="less" scoped>
.avatar {
  position: relative;

  &::after {
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    width: 100%;
    height: 100%;
    font-size: 50px;
    color: #fff;
    background-color: rgb(0 0 0 / 40%);
    border-radius: 50%;
    content: '+';
    opacity: 0;
    justify-content: center;
    align-items: center;
  }

  &:hover {
    &::after {
      opacity: 1;
    }
  }
}
</style>
