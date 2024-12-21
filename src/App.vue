<script setup lang="ts">
import {ref, onMounted} from 'vue';
const loading = ref(true);
const error = ref('');
const today = new Date();
const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
console.log(date);
const pgr1 = ref<Programs[]>([]);
interface Programs {
  id: number;
  type: string;
  name: string;
  program: string;
}
async function getProgram() {
  loading.value=true;
  if (date === '2024-12-21') {
    const pullURL = await fetch('https://api.yuanhau.com/api/events/ai-yue-wu-2024-chrismas-theme-page-list',
  {
    method: 'GET',
  }
  );
  // Backup
  if (!pullURL.ok || pullURL.status === 404 || pullURL.status === 204 || pullURL.status === 500) {
      const pullURLb = await fetch('https://api.yuanhau.com/api/ai-yue-wu/2024-chrismas.ts',
      {
        method: 'POST',
      });
      if (!pullURLb.ok || pullURLb.status === 404 || pullURLb.status === 204 || pullURLb.status === 500) {
        error.value = '系統錯誤'
        loading.value = false;
        return;
      }
      const pullData = await pullURLb.json();
      pgr1.value = pullData;
    } else {
      const pullData = await pullURL.json();
      pgr1.value = pullData;
    }
    setTimeout(() => {
    loading.value = false;
  }, 500);
  } else {
    error.value = '節目單過期'
    loading.value = false;
    return;
  }
}

onMounted(() => {
  getProgram();
  setInterval(getProgram, 1000 * 60 * 60)
})
</script>
<template>
  <nav>
    <div class="blur">
      <h1 class="title">2024 聖誕音樂會 節目單</h1>
    </div>
  </nav>
    <div v-if="loading">
      <div class="loading">
        <div class='loader'></div>
      </div>
    </div>
      <div v-if="error" class="content">
        <h2>{{ error }}</h2>
      </div>
      <div v-if="!loading && !error" class="content">
      <p v-for="(i, index) in pgr1" :key="index" class="maincontent">
        <span><span>{{i.id}}</span>. <span>{{ i.type }}</span> <span>{{ i.name }}</span> <span>{{ i.program }}</span></span>
        <br />
      </p>
      </div>
</template>
