<template>
  <div class="search-box">
    <h1>模糊搜索：</h1>
    <el-input
      prefix-icon="el-icon-search"
      v-model="searchStr"
      :clearable="true"
      placeholder="请输入内容"
      @focus="onFocus"
      @input="onInput"
      @blur="onBlur"
    ></el-input>
    <div class="lists">
      <div
        v-for="(it, index) in userLists"
        :key="index"
        class="item"
      >
        <img
          v-if="it.iconUrl !== 'null'"
          :src="it.iconUrl"
          alt=""
          class="user-icon">
        <img
          v-else
          src="../assets/icon.jpg"
          alt=""
          class="user-icon"
        >
        <div
          v-html="highLightMsg(it.username + '(' + it.userId + ')', searchStr)"
          class="user-name"
        >{{ it.username }}({{ it.userId }})</div>
      </div>
      <div v-show="showNoData && !userLists.length" class="noData">~暂无搜索结果~</div>
    </div>
  </div>
</template>

<script>
import { highLightTableMsg } from '../utils/highLight.js'
export default {
  data() {
    return {
      showNoData: true,
      searchStr: '',
      userLists: []
    }
  },
  mounted() {
  },
  methods: {
    highLightMsg(item, str) {
      return highLightTableMsg(item, str)
    },
    onFocus() {
      this.showNoData = false
      this.getAllUsers()
    },
    onInput() {
      if (!this.searchStr) {
        this.$https.get('https://www.fastmock.site/mock/8061c1aeb850aa082f2e1fb8d19d47ef/search/searchUsers')
        .then((res) => {
          this.userLists = res.data.data.userList
        })
      } else if (this.searchStr == 1) {
        this.$https.get('https://www.fastmock.site/mock/8061c1aeb850aa082f2e1fb8d19d47ef/search/searchUsersStr')
        .then((res) => {
          this.userLists = res.data.data.userList
        })
      } else {
        this.showNoData = true
        this.userLists = []
      }
    },
    onBlur() {
      this.userLists = []
      this.searchStr = ''
      this.showNoData = true
    },
    getAllUsers() {
      this.$https.get('https://www.fastmock.site/mock/8061c1aeb850aa082f2e1fb8d19d47ef/search/searchUsers')
      .then((res) => {
        this.userLists = res.data.data.userList
      })
    }
  }
}
</script>

<style scoped>
.search-box {
  width: 300px;
  height: 500px;
}
.lists {
  border-radius: 4px;
  box-shadow: 0px 0px 12px rgba(0, 0, 0, 0.18);
}
.item {
  display: flex;
  align-items: center;
  padding: 8px 12px;
}
.user-icon {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-right: 6px;
}
.user-name {
  font-size: 14px;
  color: #323232;
}
.noData {
  padding: 10px 0px;
}
</style>

