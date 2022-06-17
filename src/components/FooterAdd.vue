<script setup>
import { inject, ref } from "vue";
import { db } from "../firebase";
import { addDoc, collection } from "firebase/firestore";

const text = ref("");
const userGoogle = inject("userGoogle");

const addMessage = () => {
  addDoc(collection(db, "chats"), {
    text: text.value,
    uid: userGoogle.value.uid,
    displayName: userGoogle.value.displayName,
    time: Date.now(),
  })
    .then(() => {
      text.value = "";
    })
    .catch((e) => console.log(e));
};
</script>

<template>
  <q-footer elevated class="primary">
    <q-toolbar>
      <q-input
        dark
        dense
        class="q-ml-md full-width"
        v-model="text"
        label="Ingrese texto"
        @keyup.enter="addMessage"
      >
        <template #append>
          <q-icon
            name="send"
            class="cursor-pointer"
            type="submit"
            @click="addMessage"
          />
        </template>
      </q-input>
    </q-toolbar>
  </q-footer>
</template>
