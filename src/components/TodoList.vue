<template>
  <div class="todo-list w-[900px] flex flex-col gap-5">
    <p class="text-2xl text-center sm:text-left font-bold">TODO APP</p>
    <ul
      class="todo-status hidden sm:flex flex-wrap justify-evenly sm:justify-between items-center gap-5"
    >
      <li
        class="min-w-[100px] md:min-w-[120px] lg:min-w-[160px] text-xs border-2 rounded-md flex flex-col items-center p-3"
        v-for="(status, index) in todoStatus"
        :key="index"
      >
        {{ status.name }}
        <span class="text-3xl font-bold">{{ status.value }}</span>
      </li>
    </ul>
    <form
      class="todo-form border-2 rounded-md p-5 flex flex-col justify-center items-center gap-5"
    >
      <div class="input-section w-full flex flex-col md:flex-row gap-5">
        <div class="input-group w-full md:w-[50%] flex flex-col">
          <label for="name">Name</label>
          <input
            class="border-2 rounded-md p-2"
            type="text"
            placeholder="Nama todo"
            v-model="todoInput.name"
          />
        </div>
        <div class="input-group w-full md:w-[50%] flex flex-col">
          <label for="priority">Priority</label>
          <select
            class="border-2 rounded-md p-2"
            name="priority"
            id="priority"
            v-model="todoInput.priority"
          >
            <option value="High">High</option>
            <option value="Medium">Medium</option>
            <option value="Low">Low</option>
          </select>
        </div>
      </div>
      <button
        type="submit"
        @click="addTodo"
        class="w-full lg:w-1/5 self-end bg-green-400 border-2 border-transparent p-2 rounded-md text-white"
      >
        Save
      </button>
    </form>
    <div class="todos flex flex-wrap justify-between gap-7 lg:gap-3 w-full">
      <div class="todo-not-completed w-full lg:w-[49%]">
        <h3 class="font-bold text-xl pb-3">TODO</h3>
        <ul class="flex flex-col gap-5">
          <div class="text-center font-bold py-20" v-if="emptyTodo">
            Todo is empty
          </div>
          <li
            class="border-2 rounded-md py-4 px-3 min-h-[117px]"
            v-for="(todo, index) in todoCompleted"
            :key="index"
          >
            <div class="flex flex-col gap-5">
              <h2 class="text-lg font-semibold">{{ todo.name }}</h2>
              <div class="flex items-center justify-between">
                <div class="status flex items-center gap-1">
                  <span
                    class="w-[10px] h-[10px] block rounded-sm"
                    :class="[
                      todo.priority === 'Low'
                        ? 'bg-blue-500'
                        : todo.priority === 'Medium'
                        ? 'bg-orange-500'
                        : 'bg-red-600',
                    ]"
                  ></span
                  >{{ todo.priority }}
                </div>
                <div class="action-button flex gap-2 items-center">
                  <button
                    @click="deleteTodo(todo.id)"
                    class="py-1 px-4 bg-red-500 rounded-md text-white"
                  >
                    Delete
                  </button>
                  <button
                    @click="completeTodo(todo.id)"
                    class="py-1 px-4 bg-green-500 rounded-md text-white"
                  >
                    Complete
                  </button>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
      <div class="todo-completed w-full lg:w-[49%]">
        <h3 class="font-bold text-xl pb-3">COMPLETED</h3>
        <div
          class="text-center font-bold py-20"
          v-if="emptyCompletedTodo"
        >
          Completed Todo is empty
        </div>
        <ul class="flex flex-col gap-5">
          <li
            class="border-2 rounded-md py-4 px-3 min-h-[117px]"
            v-for="(todo, index) in todoNotCompleted"
            :key="index"
          >
            <div class="flex flex-col gap-5">
              <h2 class="text-lg font-semibold">{{ todo.name }}</h2>
              <div class="status flex items-center gap-1">
                <span
                  class="w-[10px] h-[10px] block rounded-sm"
                  :class="[
                    todo.priority === 'Low'
                      ? 'bg-blue-500'
                      : todo.priority === 'Medium'
                      ? 'bg-orange-500'
                      : 'bg-red-600',
                  ]"
                ></span
                >{{ todo.priority }}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todoList: [
        { id: 100, name: "Nonton Twice", priority: "High", isCompleted: false },
        { id: 121, name: "Ngopi", priority: "Medium", isCompleted: false },
        { id: 423, name: "Ngoding", priority: "Low", isCompleted: false },
      ],
      todoInput: {
        name: "",
        priority: "",
        isCompleted: false,
      },
    };
  },
  methods: {
    addTodo(e) {
      e.preventDefault();
      if (this.todoInput.name === "" && this.todoInput.priority === "") {
        alert("Nama todo dan prioritas tidak boleh kosong!");
      } else if (this.todoInput.name === "") {
        alert("Nama todo tidak boleh kosong!");
      } else if (this.todoInput.priority === "") {
        alert("Prioritas todo tidak boleh kosong");
      } else {
        this.todoList.push({
          id: Math.round(Math.random() * (999 - 100)),
          name: this.todoInput.name,
          priority: this.todoInput.priority,
          isCompleted: false,
        });
        this.todoInput.name = "";
        this.todoInput.priority = "";
      }
    },
    deleteTodo(id) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    },
    completeTodo(id) {
      this.todoList.map((todo) => {
        if (id === todo.id) {
          todo.isCompleted = !todo.isCompleted;
        }
      });
    },
  },
  computed: {
    todoCompleted() {
      return this.todoList.filter((todo) => !todo.isCompleted);
    },
    todoNotCompleted() {
      return this.todoList.filter((todo) => todo.isCompleted);
    },
    pendingTodo() {
      let pending = this.todoList.filter((todo) => !todo.isCompleted);
      return pending.length;
    },
    lowPriorityTodo() {
      let lowPriority = this.todoList.filter(
        (todo) => todo.priority === "Low" && !todo.isCompleted
      );
      return lowPriority.length;
    },
    mediumPriorityTodo() {
      let mediumPriority = this.todoList.filter(
        (todo) => todo.priority === "Medium" && !todo.isCompleted
      );
      return mediumPriority.length;
    },
    highPriorityTodo() {
      let highPriority = this.todoList.filter(
        (todo) => todo.priority === "High" && !todo.isCompleted
      );
      return highPriority.length;
    },
    completedTodo() {
      let completed = this.todoList.filter((todo) => todo.isCompleted);
      return completed.length;
    },
    todoStatus() {
      return [
        {
          name: "PENDING",
          value: this.pendingTodo,
        },
        {
          name: "LOW",
          value: this.lowPriorityTodo,
        },
        {
          name: "MEDIUM",
          value: this.mediumPriorityTodo,
        },
        {
          name: "HIGH",
          value: this.highPriorityTodo,
        },
        {
          name: "COMPLETED",
          value: this.completedTodo,
        },
      ];
    },
    emptyTodo() {
      return this.todoCompleted.length === 0;
    },
    emptyCompletedTodo() {
      return this.todoNotCompleted.length === 0;
    },
  },
};
</script>