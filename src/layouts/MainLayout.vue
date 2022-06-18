<template>
  <q-layout view="hHr lpR fFr">
    <navbar />

    <q-page-container>
      <router-view />
    </q-page-container>

    <footer-add v-if="userGoogle" />
  </q-layout>
</template>

<script setup>
import { inject } from "vue";
import Navbar from "src/components/Navbar.vue";
import FooterAdd from "src/components/FooterAdd.vue";

const userGoogle = inject("userGoogle");
</script>
