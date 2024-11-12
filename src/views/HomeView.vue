<template>
  <div class="max-w-lg mx-auto bg-slate-200 rounded-xl shadow-lg px-6 py-10">
    <Form
      :form="form"
      :handleToDo="handleToDo"
      :selectedTaskId="selectedTaskId"
    />
    <p>task: {{ form.task }}</p>
    <p>time: {{ form.time }}</p>
  </div>
  <h1 class="mt-6 mb-2 font-semibold">Task List</h1>
  <div
    v-if="todos.length"
    class="max-w-lg mx-auto flex flex-col items-start gap-2 px-2 py-4 rounded-md bg-white"
  >
    <Task
      :tasks="todos"
      :handleDelTask="handleDelTask"
      :handleEditTask="handleEditTask"
      :toggleTask="toggleTask"
    />
  </div>
</template>

<script>
import Form from "@/components/Form.vue";
import Task from "@/components/Task.vue";
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
          isCompleted: true,
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
      console.log("from edit task id:", selectedTaskId.value);
      const task = todos.value.find((item) => item.id === selectedTaskId.value);
      form.task = task.title;
      form.time = task.dueDate;
    };

    const handleDelTask = (id) => {
      todos.value = todos.value.filter((task) => task.id !== id);
    };

    const toggleTask = (id) => {
      console.log(id);
    };

    return {
      handleToDo,
      form,
      selectedTaskId,
      todos,
      handleEditTask,
      handleDelTask,
      toggleTask,
    };
  },
};
</script>
