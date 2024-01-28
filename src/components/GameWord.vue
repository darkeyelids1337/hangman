<script setup lang="ts">
    interface Props{
        word:string,
        correctLetters: string[]
    }

    defineProps<Props>()
</script>

<template>
  <div class="word">
    <span class="letter" v-for="(letter, key) in word" :key="key">{{ correctLetters.includes(letter) ? letter : '' }}</span>
  </div>
</template>

<style scoped></style>
