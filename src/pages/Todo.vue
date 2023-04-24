<template>
  <q-page class="q-pa-lg bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        bg-color="white"
        v-model="newTask"
        placeholder="Add Task"
        dense
        @keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        clickable
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.title"
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar @click="task.done = !task.done">
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="teal"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label @click="task.done = !task.done">{{
            task.title
          }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            color="primary"
            icon="delete"
            dense
          ></q-btn>
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "get bananas",
        //   done: false,
        // },
        // {
        //   title: "eat bananas",
        //   done: false,
        // },
        // {
        //   title: "poo bananas",
        //   done: false,
        // },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Would you like to delete this?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted");
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
  },
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.4;
}
</style>
