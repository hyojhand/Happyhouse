<template>
  <div class="w-100">
    <div class="left-content">
      <h3 class="text-light">인기글 <b-icon icon="heart"></b-icon></h3>
      <board-rank-list type="like"></board-rank-list>
      <h3>
        관심글
        <b-icon icon="chat-quote"></b-icon>
      </h3>
      <board-rank-list type="reply"></board-rank-list>
    </div>
    <div class="right-content">
      <h3 class="text-light">
        최근 게시판
        <router-link :to="{ name: 'BoardList' }" class="seemore"
          >더보기</router-link
        >
      </h3>
      <board-rank-list type="recent"></board-rank-list>
    </div>
  </div>
</template>

<script>
import BoardRankList from "@/components/board/BoardRankList.vue";
export default {
  components: {
    BoardRankList,
  },
};
</script>
<style scoped>
.left-content {
  position: absolute;
  top: 240px;
  left: 10%;
  width: 30%;
  z-index: 10;
}
.right-content {
  position: absolute;
  top: 240px;
  left: 40%;
  width: 50%;
  z-index: 10;
}
.seemore {
  font-size: 15px;
  color: ivory;
  position: absolute;
  right: 0;
  top: 10px;
}
</style>
