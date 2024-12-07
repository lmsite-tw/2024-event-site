<script setup lang="ts">
import {ref, onMounted} from 'vue';
const loading = ref(true);
const error = ref('');
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
  if (!pullURL.ok) {
    error.value = '系統錯誤'
    loading.value = false;
    return;
  }
  const pullData = await pullURL.json();
  pgr1.value = pullData;
  setTimeout(() => {
    loading.value = false;
  }, 500);
}
onMounted(getProgram);
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