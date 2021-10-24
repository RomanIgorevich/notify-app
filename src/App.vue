<template>
  <div>
    <div v-for="mes in message" :key="mes.title">
      <p v-text="mes.title" />
    </div>
    <button @click="addM">Load more</button>

  </div>
</template>

<script>
import axios from "axios";
export default {
  computed: {
    message() {
      return this.$store.getters.getMessagesMain ;
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
          console.log({messagesMain, messages})
          this.$store.dispatch("setMessages", messages);
          this.$store.dispatch("setMessagesMain", messagesMain);
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => console.log("метод завершон"));
    },
  },
};
</script>
