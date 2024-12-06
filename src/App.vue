<script setup lang="ts">
import {ref, onMounted} from 'vue';
const loading = ref(true);
const pgr1 = ref<Programs[]>([]);
interface Programs {
  id: number;
  type: string;
  name: string;
  program: string;
}
async function getProgram() {
  const pullURL = await fetch('https://v4-api.yuanhau.com/api/db/ai-yue-wu-2024-sheng-dan-yin-yue-hui',
  {
    method: 'POST',
  }
  );
  const pullData = await pullURL.json();
  pgr1.value = pullData;
  loading.value = false;
}
onMounted(getProgram);
</script>
<template>
  <nav><h1 class="title">2024 聖誕音樂會 節目單</h1></nav>
    <div v-if="loading" >
        <h2>載入中...</h2>
      </div>
      <div v-if="!loading" class="content">
      <p v-for="(i, index) in pgr1" :key="index" class="maincontent">
        <span><span>{{i.id}}</span>. <span>{{ i.type }}</span> <span>{{ i.name }}</span> <span>{{ i.program }}</span></span>
        <br />
      </p>
      </div>
</template>
<style scoped>
p.maincontent {
  font-size: 16px;
  text-align: center;
}
nav {
  background-color:#2B2D30FF;
  width:fill;
  padding:10px;
  top:0;
  left:0;
  right:0;
  margin:0;
  position:sticky;
  z-index:100;
  margin-bottom:0px;
}
h1.title {
  width:fill;
  text-align: center;
  font-size: 30px;
  margin-top:0;
  margin-bottom:0;
}
</style>