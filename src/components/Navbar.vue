<script setup>
import { ref, inject } from "vue";
import { auth } from "../firebase";
import { signInWithPopup, GoogleAuthProvider, signOut } from "firebase/auth";

import DrawerUser from "./DrawerUser.vue";

const rightDrawerOpen = ref(false);
const userGoogle = inject("userGoogle");

const toggleRightDrawer = () => {
  rightDrawerOpen.value = !rightDrawerOpen.value;
};

const accessGoogle = () => {
  const provider = new GoogleAuthProvider();
  signInWithPopup(auth, provider).catch((e) => console.log(e));
};

const logoutGoogle = () => {
  signOut(auth).catch((e) => console.log(e));
};
</script>

<template>
  <q-header elevated class="bg-primary text-white">
    <q-toolbar>
      <q-toolbar-title> QuasarChat </q-toolbar-title>
      <q-btn
        color="secondary"
        size="sm"
        @click="accessGoogle"
        v-if="!userGoogle"
        >Ingresar</q-btn
      >
      <q-btn color="secondary" size="sm" v-if="userGoogle" @click="logoutGoogle"
        >Salir</q-btn
      >

      <q-btn
        dense
        flat
        round
        icon="menu"
        @click="toggleRightDrawer"
        v-if="userGoogle"
      />
    </q-toolbar>
  </q-header>

  <q-drawer
    v-if="userGoogle"
    show-if-above
    v-model="rightDrawerOpen"
    side="right"
    bordered
  >
    <DrawerUser />
  </q-drawer>
</template>
