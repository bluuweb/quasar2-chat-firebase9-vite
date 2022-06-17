<script setup>
import { inject } from "vue";

const userGoogle = inject("userGoogle");
</script>

<template>
  <q-img
    class="absolute-top"
    src="https://cdn.quasar.dev/img/material.png"
    style="height: 150px"
  >
    <div class="absolute-bottom bg-transparent">
      <q-avatar size="56px" class="q-mb-sm">
        <img :src="userGoogle.photoURL" />
      </q-avatar>
      <div class="text-weight-bold">{{ userGoogle.displayName }}</div>
      <div>{{ userGoogle.email }}</div>
    </div>
  </q-img>
</template>
