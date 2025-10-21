<template>
  <div class="max-w-lg mx-auto py-20">
    <div class="flex items-center gap-2">
      <div class="bg-gray-300 rounded-full w-14 h-14 avatar" :class="{focus: onFocus}"><img src="./assets/photo.png" alt=""></div>
      <div>
        <label for="hours-old">{{ student.name }} is</label>
        <div class="flex gap-2 items-center">
          <NumberInput v-model="hoursValue" @focus="onFocus = true" @blur="onFocus = false"/>
          <div class="text">hours old</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import NumberInput from './NumberInput.vue';
const student = {
  name: 'Hugh',
  age: null,
  image: null,
}

const hoursValue = ref<string>('');
const onFocus = ref<boolean>(false)
</script>

<style>
.avatar {
  padding: 3px;
    border: 2px solid transparent;
    transition: border-color 0.2s ease;
}

.focus {
  border-color: #3D06D7;
}
</style>
