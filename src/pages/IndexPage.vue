<script setup>
import { collection, query, onSnapshot, orderBy } from "firebase/firestore";
import { ref, inject } from "vue";
import { db } from "../firebase";
import { useQuasar, QSpinnerFacebook } from "quasar";

const userGoogle = inject("userGoogle");
const messages = ref([]);
const chatRef = ref(null);
const $q = useQuasar();

const q = query(collection(db, "chats"), orderBy("time"));
const unsubscribe = onSnapshot(q, (snapshot) => {
  snapshot.docChanges().forEach((change) => {
    if (change.type === "added") {
      // console.log("New city: ", change.doc.data());
      messages.value.push({
        id: change.doc.id,
        ...change.doc.data(),
      });

      setTimeout(() => {
        if (chatRef.value !== null) {
          window.scrollTo(0, chatRef.value.scrollHeight);
        }
      }, 60);
    }

    $q.loading.hide();
    setTimeout(() => {
      if (chatRef.value !== null) {
        window.scrollTo(0, chatRef.value.scrollHeight);
      }
    }, 600);
  });
});

const formatDate = (time) => {
  const timeDate = new Date(time);
  return timeDate.toLocaleDateString("en-GB");
};

const showLoading = () => {
  $q.loading.show({
    spinner: QSpinnerFacebook,
    spinnerColor: "primary",
    spinnerSize: 140,
    backgroundColor: "primary",
    messageColor: "black",
  });
};

showLoading();
</script>

<template>
  <q-page class="flex flex-center" v-if="!userGoogle">
    Debes iniciar sesión
  </q-page>
  <q-page padding v-else>
    <div class="q-pa-md row justify-center" ref="chatRef">
      <div style="width: 100%; max-width: 400px">
        <template v-for="message in messages" :key="message.id">
          <q-chat-message
            :text="[message.text]"
            :sent="userGoogle.uid === message.uid"
            :name="message.displayName"
            :stamp="formatDate(message.time)"
          />
        </template>
      </div>
    </div>
  </q-page>
</template>
