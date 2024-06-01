<script setup>
    import { ref } from 'vue';

    const props = defineProps(['task'])

    const checked = ref(props.task.completed == true);
</script>

<template>
    <li class="p-2 mb-2 rounded-lg hover:p-3 flex justify-between" :class="{'bg-blue-400' : checked, 'bg-gray-400' : checked == false}">
        <p>
            <input type="checkbox" v-model="checked" @click="$emit('completed-or-not', task)">

            <span class="inline-block ml-3">{{ task.content }}</span>
        </p>

        <button type="button" @click="$emit('deleted', task)" class="bg-red-500 hover:bg-red-700 inline-block ml-2 px-2 font-bold text-white rounded-full">x</button>
    </li>
</template>