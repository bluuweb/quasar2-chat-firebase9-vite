<template>
  <q-layout view="hHr lpR fFr" v-if="userGoogle !== false">
    <Navbar />

    <q-page-container>
      <router-view />
    </q-page-container>

    <FooterAdd />
  </q-layout>
</template>

<script setup>
import { ref, provide } from "vue";
import { useQuasar, QSpinnerFacebook } from "quasar";
import { auth } from "../firebase";
import { onAuthStateChanged } from "firebase/auth";
import Navbar from "src/components/Navbar.vue";
import FooterAdd from "src/components/FooterAdd.vue";

const $q = useQuasar();
const userGoogle = ref(false);
provide("userGoogle", userGoogle);

onAuthStateChanged(auth, (user) => {
  showLoading();
  setTimeout(() => {
    userGoogle.value = user;
    $q.loading.hide();
  }, 1000);
});

const showLoading = () => {
  $q.loading.show({
    spinner: QSpinnerFacebook,
    spinnerColor: "primary",
    spinnerSize: 140,
    backgroundColor: "primary",
    messageColor: "black",
  });
};
</script>
