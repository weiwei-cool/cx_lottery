<template>
  <div class="container text-center mt-5">
    <h1>创新命运的轨迹</h1> <p class="text-muted">Power by StelaHaveno</p>
    <div class="d-flex justify-content-center mt-4">
      <button class="btn btn-primary mx-2" @click="drawReward">奖励</button>
      <button class="btn btn-danger mx-2" @click="drawPunishment">惩罚</button>
      <button class="btn btn-warning mx-2" @click="drawMixed">有奖有罚</button>
    </div>
    <div class="text-end mt-3">
      <a href="#" @click="toggleSettings" class="btn btn-primary">设置</a>
    </div>

    <div v-if="isSettingsOpen" class="mt-4">
      <h2>设置</h2>
      <div class="row">
        <div class="col-md-6">
          <h3>奖励</h3>
          <ul class="list-group">
            <li v-for="(reward, index) in rewards" :key="index" class="list-group-item d-flex justify-content-between">
              {{ reward }}
              <button class="btn btn-sm btn-outline-danger" @click="removeReward(index)">删除</button>
            </li>
          </ul>
          <input v-model="newReward" class="form-control mt-2" placeholder="添加奖励">
          <button class="btn btn-primary mt-2" @click="addReward">添加奖励</button>
        </div>
        <div class="col-md-6">
          <h3>惩罚</h3>
          <ul class="list-group">
            <li v-for="(punishment, index) in punishments" :key="index" class="list-group-item d-flex justify-content-between">
              {{ punishment }}
              <button class="btn btn-sm btn-outline-danger" @click="removePunishment(index)">删除</button>
            </li>
          </ul>
          <input v-model="newPunishment" class="form-control mt-2" placeholder="添加惩罚">
          <button class="btn btn-danger mt-2" @click="addPunishment">添加惩罚</button>
        </div>
      </div>
    </div>

    <div v-if="result" class="mt-4">
      <h3>抽取结果</h3>
      <div class="alert alert-info" role="alert">{{ result }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const rewards = ref(['饮料一瓶', '零食一份', '免一张卷子']);
const punishments = ref(['跑800米', '800字作文', '打扫教室一次']);

const newReward = ref('');
const newPunishment = ref('');

const result = ref('');
const isSettingsOpen = ref(false);

const toggleSettings = () => {
  isSettingsOpen.value = !isSettingsOpen.value;
};

const addReward = () => {
  if (newReward.value.trim()) {
    rewards.value.push(newReward.value.trim());
    newReward.value = '';
  }
};

const removeReward = (index) => {
  rewards.value.splice(index, 1);
};

const addPunishment = () => {
  if (newPunishment.value.trim()) {
    punishments.value.push(newPunishment.value.trim());
    newPunishment.value = '';
  }
};

const removePunishment = (index) => {
  punishments.value.splice(index, 1);
};

const drawReward = async () => {
  if (rewards.value.length) {
    await wait();
    result.value = `抽取结果·奖励: ${randomItem(rewards.value)}`;
  } else {
    result.value = '没有可用的奖励!';
  }
};

const drawPunishment = async () => {
  if (punishments.value.length) {
    await wait();
    result.value = `抽取结果·惩罚: ${randomItem(punishments.value)}`;
  } else {
    result.value = '没有可用的惩罚!';
  }
};

const drawMixed = async () => {
  const combined = [...rewards.value, ...punishments.value];
  if (combined.length) {
    await wait();
    result.value = `抽取结果·有奖有罚: ${randomItem(combined)}`;
  } else {
    result.value = '没有可用的奖励或惩罚!';
  }
};

async function wait() {
  result.value = '命运的答案即将揭晓！';
  await sleep(2000);
  result.value = '3!';
  await sleep(1000);
  result.value = '2!';
  await sleep(1000);
  result.value = '1!';
  await sleep(1000);
}

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const randomItem = (array) => {
  const randomIndex = Math.floor(Math.random() * array.length);
  return array[randomIndex];
};
</script>

<style scoped>
.container {
  max-width: 800px;
}
</style>
