<template>
  <div class="header">
    <!-- 面包屑组件 -->
    <Breadcrumb />
    <el-dropdown>
      <span class="el-dropdown-link">
        <el-avatar
          shape="square"
          :size="40"
          :src="userInfo.portrait || 'https://public.shiguanghai.top/blog_img/default-avatarnUSD98.png'"
        ></el-avatar>
        <i class="el-icon-arrow-down el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item>{{ userInfo.userName }}</el-dropdown-item>
        <el-dropdown-item
          divided
          @click.native="handleLogout"
        >退出</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { getUserInfo } from '@/services/user'
import Breadcrumb from '@/components/Breadcrumb/index.vue'

export default Vue.extend({
  name: 'AppHeader',
  components: {
    Breadcrumb
  },
  data () {
    return {
      userInfo: {} // 当前登录用户信息
    }
  },
  created () {
    this.loadUserInfo()
    // console.log('router.currentRoute.matched', this.$route.matched)
  },
  methods: {
    async loadUserInfo () {
      const { data } = await getUserInfo()
      this.userInfo = data.content
      // console.log('loadUserInfo')
    },
    handleLogout () {
      this.$confirm('确认退出?', '退出提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 清除用户登录转态
        this.$store.commit('setUser', null)

        // 跳转到登录页面
        this.$router.push({
          name: 'login'
        })
        this.$message({
          type: 'success',
          message: '退出成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消退出'
        })
      })
    }
  }
})
</script>

<style lang="scss" scoped>
.header {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  .el-dropdown-link {
    display: flex;
    align-items: center;
  }
}
</style>
