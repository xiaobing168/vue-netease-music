<template>
  <div class="header">
    <div class="left">
      <div
        @click="onClickLogo"
        class="logo-wrap"
      >
        <img
          class="logo"
          :src="logo"
        />
        <span>云音乐</span>
      </div>
      <div
        @click="onClickDown"
        v-if="isPlayerShow"
      >
        <Icon type="down" />
      </div>
      <div
        v-else
        class="nav-wrap"
      >
        <Tabs :tabs="tabs" />
      </div>
    </div>
    <div class="right">
      <div class="search-wrap">
        <Search />
      </div>
      <Theme />
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import logo from "@/assets/logo.png"
import Theme from "@/components/theme"
import Search from "@/components/search"
import { mapState, mapMutations } from "vuex"

export default {
  created() {
    this.logo = logo
    this.tabs = [
      {
        title: "个性推荐",
        to: "/discovery"
      },
      {
        title: "歌单",
        to: "/playlists"
      },
      {
        title: "最新音乐",
        to: "/songs"
      }
    ]
  },
  methods: {
    onClickLogo() {
      this.$router.push("/discovery")
    },
    onClickDown() {
      this.setPlayerShow(false)
    },
    ...mapMutations(["setPlayerShow"])
  },
  computed: {
    ...mapState(["isPlayerShow"])
  },
  components: { Theme, Search }
}
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: $header-height;
  background-color: var(--header-bgcolor);
  padding: 16px;
  padding-right: 36px;

  .left {
    display: flex;
    align-items: center;

    .logo-wrap {
      display: flex;
      align-items: center;
      margin-right: 36px;
      font-size: $font-size-title;
      color: var(--font-color-white);
      white-space: nowrap;
      cursor: pointer;

      .logo {
        width: 30px;
        height: 30px;
        margin-right: 8px;
      }
    }

    .font-weight {
      white-space: nowrap;
    }
  }

  .right {
    display: flex;
    align-items: center;

    .search-wrap {
      margin-right: 16px;
    }
  }
}
</style>
