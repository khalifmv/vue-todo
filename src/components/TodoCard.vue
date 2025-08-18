<script setup>
const props = defineProps({
  todo: {
    id: Number,
    text: String,
    completed: Boolean
  }
});
const emit = defineEmits(['toggle', 'edit' ,'delete']);
</script>

<template>
    <div class="grid grid-cols-12 gap-2 p-2 border border-gray-300 rounded-lg bg-white">
        <div class="col-span-1 flex items-center justify-center">
            <input v-model="props.todo.completed" @change="emit('toggle', props.todo.id, props.todo.completed)" type="checkbox" class="cursor-pointer" />
        </div>
        <div class="col-span-8 md:col-span-8">
            <h2 class="text-md text-gray-600 truncate" :class="{'line-through': props.todo.completed}">{{ props.todo.text }}</h2>
        </div>

        <div class="col-span-3 md:col-span-3 flex justify-end items-center gap-3 pr-2 hover:opacity-30">
            <img @click="emit('edit', props.todo.id)" src="/edit.svg" alt="Edit" class="cursor-pointer w-4 h-4" />
            <img @click="emit('delete', props.todo.id)" src="/x-symbol.svg" alt="Edit" class="cursor-pointer w-3 h-3" />
        </div>

    </div>
</template>

<style scoped></style>