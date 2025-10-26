<template>
  <div class="navbar">
    <!-- 左侧菜单 -->
    <hamburger id="hamburger-container" :is-active="sidebar.opened" class="hamburger-container" @toggleClick="toggleSideBar" />

    <!-- 面包屑导航 -->
    <breadcrumb id="breadcrumb-container" class="breadcrumb-container" />

    <!-- 右侧菜单 -->
    <div class="right-menu">
      <template v-if="device !== 'mobile'">
        <!-- 搜索 -->
        <search id="header-search" class="right-menu-item" :placeholder="labels.searchPlaceholder" />

        <!-- 错误日志 -->
        <error-log class="errLog-container right-menu-item hover-effect" />

        <!-- 全屏切换 -->
        <screenfull id="screenfull" class="right-menu-item hover-effect" />

        <!-- 全局尺寸选择 -->
        <el-tooltip :content="labels.globalSizeTooltip" effect="dark" placement="bottom">
          <size-select id="size-select" class="right-menu-item hover-effect" />
        </el-tooltip>

        <!-- 语言切换 -->
        <el-dropdown class="right-menu-item hover-effect" @command="toggleLanguage">
          <span class="el-dropdown-link">
            <i class="el-icon-translate" style="margin-right: 5px;" />
            {{ currentLanguageLabel }}
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="en">English</el-dropdown-item>
            <el-dropdown-item command="zh">中文</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </template>

      <!-- 用户头像和下拉菜单 -->
      <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div class="avatar-wrapper">
          <img :src="avatar + '?imageView2/1/w/80/h/80'" class="user-avatar" />
          <i class="el-icon-caret-bottom" />
        </div>
        <el-dropdown-menu slot="dropdown">
          <router-link to="/profile/index">
            <el-dropdown-item>{{ labels.profile }}</el-dropdown-item>
          </router-link>
          <router-link to="/theme/index">
            <el-dropdown-item>{{ labels.theme }}</el-dropdown-item>
          </router-link>
          <router-link to="/">
            <el-dropdown-item>{{ labels.dashboard }}</el-dropdown-item>
          </router-link>
          <a target="_blank" href="https://github.com/PanJiaChen/vue-element-admin/">
            <el-dropdown-item>Github</el-dropdown-item>
          </a>
          <a target="_blank" href="https://panjiachen.github.io/vue-element-admin-site/#/">
            <el-dropdown-item>{{ labels.docs }}</el-dropdown-item>
          </a>
          <el-dropdown-item divided @click.native="logout">
            <span style="display: block;">{{ labels.logout }}</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import Breadcrumb from '@/components/Breadcrumb';
import Hamburger from '@/components/Hamburger';
import ErrorLog from '@/components/ErrorLog';
import Screenfull from '@/components/Screenfull';
import SizeSelect from '@/components/SizeSelect';
import Search from '@/components/HeaderSearch';

export default {
  components: {
    Breadcrumb,
    Hamburger,
    ErrorLog,
    Screenfull,
    SizeSelect,
    Search,
  },
  data() {
    return {
      avatar: 'https://wpimg.wallstcn.com/69a1c46c-eb1c-4b46-8bd4-e9e686ef5251.png',
      currentLanguage: 'en', // 默认语言
      languageLabels: {
        en: {
          currentLanguageLabel: 'English',
          searchPlaceholder: 'Search...',
          globalSizeTooltip: 'Global Size',
          profile: 'Profile',
          dashboard: 'Dashboard',
          docs: 'Docs',
          logout: 'Log Out',
          theme: 'Theme',
        },
        zh: {
          currentLanguageLabel: '中文',
          searchPlaceholder: '搜索...',
          globalSizeTooltip: '全局大小',
          profile: '个人资料',
          dashboard: '仪表盘',
          docs: '文档',
          logout: '退出登录',
          theme:'主题'
        },
      },
    };
  },
  computed: {
    ...mapGetters(['sidebar', 'device']),
    labels() {
      return this.languageLabels[this.currentLanguage];
    },
    currentLanguageLabel() {
      return this.labels.currentLanguageLabel;
    },
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('app/toggleSideBar');
    },
    async logout() {
      await this.$store.dispatch('user/logout');
      this.$router.push(`/login?redirect=${this.$route.fullPath}`);
    },
    toggleLanguage(language) {
      this.currentLanguage = language; // 更新当前语言
    },
  },
};
</script>

<style lang="scss" scoped>
.navbar {
  height: 50px;
  overflow: hidden;
  position: relative;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background 0.3s;
    -webkit-tap-highlight-color: transparent;

    &:hover {
      background: rgba(0, 0, 0, 0.025);
    }
  }

  .breadcrumb-container {
    float: left;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 50px;

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;

      &.hover-effect {
        cursor: pointer;
        transition: background 0.3s;

        &:hover {
          background: rgba(0, 0, 0, 0.025);
        }
      }
    }

    .avatar-container {
      margin-right: 30px;

      .avatar-wrapper {
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }

    .el-icon-translate {
      font-size: 18px;
    }
  }
}
</style>
