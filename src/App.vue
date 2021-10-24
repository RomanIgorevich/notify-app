<template>
  <div>
    <button @click="addM">click</button>
    <div v-for="mes in message" :key="mes.title">
      <p v-text="mes.title" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  computed: {
    message() {
      return this.$store.getters.getMessage;
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
          this.$store.dispatch("setMessage", r.data);
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => console.log("метод завершон"));
    },
  },
};
</script>
