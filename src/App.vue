<template>
  <div>
    <div v-for="mes in messagesMain" :key="mes.title">
      <p v-text="mes.title" />
    </div>
    <button
      @click="loadMore"
      :disabled="maxLength === 0"
      :class="{ btnDisabled: maxLength === 0 }"
    >
      Load more
    </button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  computed: {
    messagesMain() {
      return this.$store.getters.getMessagesMain;
    },
    maxLength() {
      return this.$store.getters.getMessageFilter.length;
    },
  },
  mounted() {
    this.addM();
  },
  methods: {
    addM() {
      axios
        .get("http://localhost:3000/message")
        .then((r) => {
          let res = r.data,
            messages = [],
            messagesMain = [];
          for (let i = 0; i < res.length; i++) {
            if (res[i].main) messagesMain.push(res[i]);
            else messages.push(res[i]);
          }
          console.log({ messagesMain, messages });
          this.$store.dispatch("setMessages", messages);
          this.$store.dispatch("setMessagesMain", messagesMain);
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => console.log("метод завершон"));
    },
    loadMore() {
      console.log("t");
      this.$store.dispatch("loadMessages").catch((err) => {
        console.log(err);
      });
    },
  },
};
</script>

<style>
.btnDisabled {
  cursor: default;
  opacity: 0.6;
}
</style>
