<script setup>
import { onBeforeMount, ref } from 'vue';
import MyButton from '../../components/button/MyButton.vue';
import { usePostIndexStore } from '../../store/post/userPostIndexStore.js';
import { useRouter } from 'vue-router';

const router = useRouter();
const postIndexStore = usePostIndexStore();

const getNextPage = async () => {
  await postIndexStore.getPostPagination(postIndexStore.getNextPageNumber);
}

const redirectShow = (id) => {
  router.push(`/posts/${id}`);
}

// 라이프 사이클
onBeforeMount(postIndexStore.getPostPagination);
onBeforeMount(postIndexStore.clearPostIndex);
</script>

<template>
<div class="card-container">
  <div
    class="card" 
    v-for="item in postIndexStore.items"
    :key="item.id"
    :style="{backgroundImage: `url(${item.image})`}"
    @click="redirectShow(item.id)"
  ></div>
</div>
<MyButton 
  v-if="!postIndexStore.isLastPage"
  :color="'gray'"
  :size="'big'"
  :content="'Show more post from mk'"
  @click="getNextPage()"
/>

</template>

<style scoped>
.card-container {
  padding: 10px;
  gap: 10px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); 
}

.card {
  padding-top: 100%;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  border-radius: 10px;
}
</style>
