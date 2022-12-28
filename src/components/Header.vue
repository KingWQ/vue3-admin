<template>
    <div class="header">
        <div class="left">
            <span style="font-size: 20px">{{ state.name }}</span>
        </div>

        <div class="right">
            <el-popover
                placement="bottom"
                :width="320"
                trigger="click"
                poper-class="poper-user-box"
            >
                <template #reference>
                    <div class="author">
                        <el-icon><Avatar /></el-icon>
                        {{ state.userInfo && state.userInfo.nickName || '' }}
                        <el-icon><CaretBottom /></el-icon>
                    </div>
                </template>

                <div class="nickname">
                    <p>登录名：{{ state.userInfo && state.userInfo.loginUserName || '' }}</p>
                    <p>昵称：{{ state.userInfo && state.userInfo.nickName || '' }}</p>
                    <el-tag size="small" effect="dark" class="logout" @click="logout">退出</el-tag>
                </div>
            </el-popover>
        </div>
    </div>
</template>

<script setup>
import { onMounted, reactive, toRefs } from 'vue'
import { useRouter } from 'vue-router'
import axios from '@/utils/axios'
import { localRemove, pathMap } from '@/utils'

const router = useRouter()

const state = reactive({
    name: 'dashboard',
    userInfo: null,
})

// 初始执行方法
onMounted(()=>{
    const pathname = window.location.hash.split('/')[1] || ''
    console.log(window.location.hash)
    if(!['login'].includes(pathname)){
        getUserInfo()
    }
})

// 获取用户信息
const getUserInfo = async()=>{
    const userInfo = await axios.get('/adminUser/profile')
    state.userInfo = userInfo
}

// 退出登录
const logout = () => {
    axios.delete('/logout').then(()=>{
        // 退出之后，将本地保存的token清理掉
        localRemove('token')
        router.push({ path: '/login' })
    })
}

// 监听路由变化方法 afterEach
router.afterEach((to)=>{
    const { id } = to.query
    state.name = pathMap[to.name]
})
</script>

<style scoped>
.header{
    height: 50px;
    border-bottom: 1px solid #e9e9e9;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 20px;
}
.author{
    margin-left:10px;
    cursor: pointer;
}
</style>
<style>
  .popper-user-box {
    background: url('https://s.yezgea02.com/lingling-h5/static/account-banner-bg.png') 50% 50% no-repeat!important;
    background-size: cover!important;
    border-radius: 0!important;
  }
   .popper-user-box .nickname {
    position: relative;
    color: #ffffff;
  }
  .popper-user-box .nickname .logout {
    position: absolute;
    right: 0;
    top: 0;
    cursor: pointer;
  }
</style>