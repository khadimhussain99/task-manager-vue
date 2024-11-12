<template>
  <form class="w-full flex flex-col gap-2">
    <input
      class="flex-grow rounded-md px-3 py-2 outline-none text-xs text-black"
      type="text"
      placeholder="write your task here..."
      v-model="form.task"
    />
    <input
      class="flex-grow rounded-md px-3 py-2 outline-none text-xs text-black"
      placeholder="select complete date"
      type="datetime-local"
      v-model="form.time"
    />
    <button
      class="ml-auto px-4 py-2 rounded-md bg-green-400 active:bg-green-300 text-white font-medium text-xs shadow-inner"
      @click="handleToDo"
    >
      <span class="flex items-center gap-2">{{
        selectedTaskId ? "Edit" : "Add"
      }}</span>
    </button>
  </form>
</template>

<script>
export default {
  name: "Form",
  props: ["form", "selectedTaskId", "handleToDo"],
};
</script>
