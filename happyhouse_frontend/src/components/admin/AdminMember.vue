<template>
  <b-container class="mt-3">
    <div class="container text-center mt-3">
      <div class="col-lg-8 mx-auto">
        <h2 class="p-3 mb-3 text-light">회원 관리</h2>
        <b-row class="search-row">
          <b-form-input
            type="text"
            class="mr-3 w-300"
            v-model="keyword"
            placeholder="검색어를 입력하세요"
            @keyup.enter="search"
          ></b-form-input>
          <b-button @click="search"><b-icon icon="search"></b-icon></b-button>
        </b-row>
        <table class="text-light w-100 mt-100">
          <tr class="admin-table-head">
            <td>회원 아이디</td>
            <td>회원 이름</td>
            <td>회원 이메일</td>
            <td>가입일</td>
          </tr>
          <tr v-for="user in users" :key="user.userid" class="admin-table-body">
            <td>{{ user.userid }}</td>
            <td>{{ user.username }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.joindate }}</td>
            <td>
              <b-button size="sm" @click="checkDelete(user.userid)"
                >삭제</b-button
              >
            </td>
          </tr>
        </table>
      </div>
    </div>
  </b-container>
</template>

<script>
import http from "@/util/http-common";
export default {
  name: "AdminMember",
  data() {
    return {
      users: [
        {
          userid: "",
          username: "",
          email: "",
          joindate: "",
        },
      ],
      keyword: "",
    };
  },
  created() {
    http.get(`/admin/member`).then(({ data }) => {
      console.log(data);
      this.users = data;
    });
  },
  methods: {
    moveBoard(no) {
      this.$router.push({
        name: "BoardView",
        params: { no: no },
      });
    },
    checkDelete(userid) {
      if (confirm("회원을 강제 탈퇴시키시겠습니까?")) {
        this.deleteUser(userid);
      }
    },
    deleteUser(userid) {
      http.delete(`/member/${userid}`).then(({ data }) => {
        if (data === "success") {
          alert("탈퇴 완료!");
          http.get(`/admin/member`).then(({ data }) => {
            console.log(data);
            this.users = data;
          });
        } else {
          alert("탈퇴 중 오류가 발생했습니다.");
        }
      });
    },
    search() {
      if (this.keyword == "") {
        http.get(`/admin/member`).then(({ data }) => {
          this.users = data;
        });
      } else {
        http.get(`admin/member/search/${this.keyword}`).then(({ data }) => {
          this.users = data;
        });
      }
    },
  },
};
</script>

<style scoped>
.search-row {
  position: absolute;
  right: 0;
}
.w-300 {
  width: 300px;
}
.mt-100 {
  margin-top: 100px;
}
</style>
