<template>
  <div class="container">
    <h2 class="text-center mt-5">RASPORED ISPITA</h2>

    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Dodaj kolegij"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        DODAJ
      </button>
    </div>
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">KOLEGIJ</th>
          <th scope="col">ROK</th>
          <th scope="col" class="text-center"></th>
          <th scope="col" class="text-center"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th>
            <span
              :class="{
                ljetni: task.rok === 'ljetni',
                zimski: task.rok === 'zimski',
                jesenski: task.rok === 'jesenski',
              }"
              >{{ task.kolegij }}</span
            >
          </th>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                zimski: task.rok === 'zimski',
                ljetni: task.rok === 'ljetni',
                jesenski: task.rok === 'jesenski',
              }"
              >{{ task.rok }}</span
            >
          </td>
          <td class="pointer">
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td class="pointer">
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["ljetni", "zimski", "jesenski"],

      tasks: [
        {
          kolegij: "IKI",
          rok: "ljetni",
        },
        {
          kolegij: "P2",
          rok: "zimski",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          kolegij: this.task,
          rok: "ljetni",
        });
      } else {
        this.tasks[this.editedTask].kolegij = this.task;
        this.editedTask = null;
      }
      this.task = "";
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].kolegij;
      this.editedTask = index;
    },

    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].rok);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].rok = this.availableStatuses[newIndex];
    },
  },
};
</script>

<style scoped>
* {
  font-family: monospace;
}
.btn-warning {
  color: #000;
  background-color: lightgray;
  border-color: black;
}

.pointer {
  cursor: pointer;
}

.ljetni {
  color: magenta;
}

.zimski {
  color: teal;
}

.jesenski {
  color: tomato;
}
</style>
