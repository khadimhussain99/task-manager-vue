<template>
  <div class="max-w-lg mx-auto bg-slate-200 rounded-xl shadow-lg px-6 py-10">
    <Form
      :form="form"
      :handleToDo="handleToDo"
      :selectedTaskId="selectedTaskId"
    />
  </div>
  <div class="max-w-lg mx-auto rounded-md bg-slate-50 p-2 mt-6">
    <h1 class="mt-2 mb-2 font-semibold text-gray-800">Task List</h1>
    <div class="flex items-center rounded-md p-2 shadow-md bg-slate-200">
      <input
        class="flex-grow w-full px-3 py-2 outline-none text-xs text-black bg-transparent"
        type="text"
        placeholder="Search task here.."
        v-model="search.text"
      />
      <select
        class="text-xs px-2 py-1 rounded outline-none text-gray-700"
        v-model="search.option"
      >
        <option value="">All</option>
        <option value="1">Completed</option>
        <option value="2">Pending</option>
      </select>
    </div>
    {{ search.options }}
    <div v-if="todos.length" class="flex flex-col items-start gap-2 px-2 py-4">
      <Task
        :tasks="searchedTasks"
        :handleDelTask="handleDelTask"
        :handleEditTask="handleEditTask"
      />
    </div>
  </div>
</template>

<script>
import Form from "@/components/Form.vue";
import Task from "@/components/Task.vue";
import { computed } from "vue";
import { onUpdated } from "vue";
import { onMounted } from "vue";
import { reactive, ref } from "vue";

export default {
  name: "HomeView",
  components: {
    Task,
    Form,
  },
  setup() {
    const todos = ref([]);
    const form = reactive({ task: "", time: "" });
    const search = ref({ text: "", option: "" });

    let selectedTaskId = ref(null);

    const handleToDo = (e) => {
      e.preventDefault();
      if (form.task === "" || form.time === "") {
        alert("please fill all fields");
        return;
      }

      if (!selectedTaskId.value) {
        const item = {
          title: form.task,
          isCompleted: false,
          dueDate: form.time,
          id: Math.floor(Math.random() * 1000),
        };
        todos.value.push(item);
      } else {
        todos.value.forEach((item) => {
          if (item.id === selectedTaskId.value) {
            item.title = form.task;
            item.dueDate = form.time;
            return;
          }
        });
        selectedTaskId.value = null;
      }
      form.task = "";
      form.time = "";
    };

    const handleEditTask = (id) => {
      selectedTaskId.value = id;

      const task = todos.value.find((item) => item.id === selectedTaskId.value);
      form.task = task.title;
      form.time = task.dueDate;
    };

    const handleDelTask = (id) => {
      todos.value = todos.value.filter((task) => task.id !== id);
    };

    const searchedTasks = computed(() => {
      return search.value
        ? todos.value.filter((item) => {
            if (Number(search.value.option) === 1) {
              return item.title.includes(search.value.text) && item.isCompleted;
            } else if (Number(search.value.option) === 2) {
              return (
                item.title.includes(search.value.text) && !item.isCompleted
              );
            } else {
              return item.title.includes(search.value.text);
            }
          })
        : todos.value;
    });

    onMounted(() => {
      const tasks = localStorage.getItem("tasks");
      todos.value = tasks ? JSON.parse(tasks) : [];
    });

    onUpdated(() => {
      const stringTasks = JSON.stringify(todos.value);
      localStorage.setItem("tasks", stringTasks);
    });

    return {
      handleToDo,
      form,
      selectedTaskId,
      todos,
      handleEditTask,
      handleDelTask,
      searchedTasks,
      search,
    };
  },
};
</script>
