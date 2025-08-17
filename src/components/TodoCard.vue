<script setup>
const props = defineProps({
  todo: {
    id: Number,
    text: String,
    completed: Boolean
  }
});
const emit = defineEmits(['toggle', 'delete']);
</script>

<template>
    <div class="grid grid-cols-12 gap-2 p-2 border border-gray-300 rounded-lg bg-white">
        <div class="col-span-1 flex items-center justify-center">
            <input v-model="props.todo.completed" @change="emit('toggle', props.todo.id, props.todo.completed)" type="checkbox" class="cursor-pointer" />
        </div>
        <div class="col-span-10">
            <h2 class="text-md text-gray-600" :class="{'line-through': props.todo.completed}">{{ props.todo.text }}</h2>
        </div>

        <div class="cols-span-1 flex justify-end items-center pr-2 hover:opacity-30">
            <img @click="emit('delete', props.todo.id)" src="/x-symbol.svg" alt="Edit" class="cursor-pointer w-3 h-3" />
        </div>

    </div>
</template>

<style scoped></style>