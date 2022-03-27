<template>
  <v-container class="main text-center">
    <h1 class="title">To-do list</h1>
    <form class="form">
      <v-text-field
        label="Input your next task"
        hide-details="auto"
        v-model="todo"
        v-on:keydown.enter.prevent="addTasks"
      ></v-text-field>
    </form>

    <section class="items">
      <div v-for="task in tasks" :key="task" class="items-list">
        <p>{{ task.text }}</p>
        <v-btn class="items-button" @click="finish(task.id, task.text)"
          >Done</v-btn
        >
      </div>
    </section>

    <section class="items">
    <v-btn v-if="!showFinished" @click="showFinished = !showFinished">Show finished</v-btn>
    <v-btn v-else @click="showFinished = !showFinished">Hide finished</v-btn> 
    <!-- existuje alternativa použitím jen 1 tlačítka? -->
    <div v-if="showFinished">
      <div v-for="task in finished" :key="task" class="items-list">
        <p>{{ task.text }}</p>
      </div>
    </div>
    </section>
  </v-container>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "HelloWorld",

  data() {
    return {
      todo: "",
      showFinished: false,
      tasks: [
        { id: 1, text: "buy milk" },
        { id: 2, text: "call boss" }, 
      ],
      finished: [{ id: 0, text: "" }], 
      //neviditelný prvek array, 
      //potřebuju zjistit jak se předdefinuje array bez obsahu
      //mám na mysli například prázdný array s kontrukcí [{id: number, text: string}]
    };
  },

  methods: {
    addTasks() {
      this.tasks.push({ id: this.tasks.length + 1, text: this.todo });
      this.todo = "";
    },
    finish(taskId: number, taskText: string) {
      this.finished.push({ id: this.finished.length + 1, text: taskText });
      this.tasks = this.tasks.filter((obj) => {
        return obj.id !== taskId;
      });
    },
  },
  //možná sáhnout si více po lifecyklu, nejsem si u nich moc jistý
});
</script>

<style lang="scss" scoped>
.main {
  display: relative;
}
.form {
  display: flex;
  flex-flow: column;
  max-width: 50%;
  margin: auto auto;
}
.items {
  margin-bottom: 50px;
  &-list {
    display: flex;
    flex-flow: row;
    align-items: center;
    justify-content: center;
    padding: 10px;
  }
  &-button {
    margin-left: 10px;
  }
}
</style>
