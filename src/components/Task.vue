<template>
  <div
    class="w-full flex justify-between items-center px-2 py-1 bg-slate-100 rounded-md"
    v-for="item in tasks"
    :key="item.id"
  >
    <input
      type="checkbox"
      value="item.isCompleted"
      v-model="item.isCompleted"
    />

    <p
      class="flex grow text-ellipsis line-clamp-1 text-xs text-black ml-2"
      :class="{
        'line-through text-gray-500 cursor-not-allowed': item.isCompleted,
      }"
    >
      {{ item.title }}
    </p>
    <span>{{ item.dueDate }}</span>
    <button
      class="rounded-md bg-orange-400 mr-2 active:bg-orange-300 text-white px-3 py-2 text-xs"
      :class="{ 'cursor-not-allowed': item.isCompleted }"
      @click="handleEditTask(item.id)"
    >
      <Icon icon="iconamoon:edit-light" />
    </button>
    <button
      class="rounded-md bg-orange-400 active:bg-orange-300 text-white px-3 py-2 text-xs"
      @click="handleDelTask(item.id)"
    >
      <Icon icon="fluent-mdl2:delete" />
    </button>
  </div>
</template>

<script>
import { Icon } from "@iconify/vue";
// import moment from moment-timezone;

export default {
  name: "TaskList",
  components: { Icon },
  props: [
    "tasks",
    "handleEditTask",
    "handleDelTask",
    "handleComplete",
    "toggleTask",
  ],
};
</script>
