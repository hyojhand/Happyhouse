<template>
  <div style="position: absolute; z-index: 99; right: 0">
    <b-button v-b-toggle.my-sidebar-right class="mt-3" id="three-dots-btn"
      ><b-icon icon="three-dots-vertical" class="three-dots-icon"></b-icon
    ></b-button>
    <b-sidebar id="my-sidebar-right" right shadow>
      <h5 v-if="isLogin" style="float: right; margin-right: 20px">
        {{ this.userInfo.username }}님 반갑습니다 :&#41;
      </h5>
      <div class="px-2 py-5">
        <ul>
          <div v-if="isLogin">
            <li>
              <router-link :to="{ name: 'Mypage' }"
                ><b-icon icon="person-lines-fill"></b-icon>
                마이페이지</router-link
              >
            </li>
            <li>
              <router-link
                :to="{
                  name: 'HouseList',
                  params: { type: 'bookmark' },
                }"
                ><b-icon icon="bookmark"></b-icon> 북마크</router-link
              >
            </li>
            <li>
              <a href="#" class="link" @click="logout"
                ><b-icon icon="person-circle"></b-icon> 로그아웃
              </a>
            </li>
          </div>
          <div v-else>
            <li>
              <router-link :to="{ name: 'SignIn' }" class="link"
                ><b-icon icon="key"></b-icon> 로그인</router-link
              >
            </li>
            <li>
              <router-link :to="{ name: 'SignUp' }" class="link"
                ><b-icon icon="person-circle"></b-icon> 회원가입</router-link
              >
            </li>
          </div>
        </ul>
        <ul>
          <li>
            <router-link :to="{ name: 'Home' }"
              ><b-icon icon="house"></b-icon> 홈</router-link
            >
          </li>
          <li>
            <router-link :to="{ name: 'Board' }"
              ><b-icon icon="file-text"></b-icon> 게시판</router-link
            >
          </li>
          <li>
            <router-link :to="{ name: 'House' }"
              ><b-icon icon="geo-alt"></b-icon> 아파트 정보</router-link
            >
          </li>
        </ul>
        <ul v-if="isLogin && isadmin">
          <li>
            <router-link :to="{ name: 'Admin' }">관리자 페이지</router-link>
          </li>
        </ul>
      </div>
    </b-sidebar>
  </div>
</template>

<script>
import { mapState, mapActions } from "vuex";
import http from "@/util/http-common";
import jwt_decode from "jwt-decode";
export default {
  name: "NaviBar",
  data() {
    return {
      userInfo: [],
    };
  },
  created() {
    this.$nextTick(function () {
      if (this.isLogin) {
        const decode = jwt_decode(sessionStorage.getItem("access-token"));
        http.defaults.headers["access-token"] =
          sessionStorage.getItem("access-token");
        http.get(`/member/info/${decode.userid}`).then(({ data }) => {
          this.userInfo = data.userInfo;
          console.log(this.userInfo);
        });
      }
    });
  },
  computed: {
    ...mapState("memberStore", ["isLogin", "isadmin"]),
  },
  methods: {
    ...mapActions("memberStore", ["setIsNotLogin", "setIsNotAdmin"]),
    logout() {
      console.log("로그아웃 시작");
      sessionStorage.removeItem("access-token");
      this.setIsNotLogin();
      this.setIsNotAdmin();
      alert("로그아웃 성공!");
      this.$router.push({ name: "Home" });
    },
  },
};
</script>

<style>
#three-dots-btn {
  background-color: transparent;
  border: transparent;
  box-shadow: none;
}
.three-dots-icon {
  width: 30px;
  height: 30px;
  color: pink;
}
</style>
