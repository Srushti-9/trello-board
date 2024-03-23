<script setup lang="ts">
    import { nanoid } from "nanoid";
    import draggableComponent from "vuedraggable";
    import {ref} from "vue";
    import { Column } from '~/types';
    import TrelloBoardTask from "./TrelloBoardTask.vue";
    const columns = ref<Column[]>([
        {
            id: nanoid(),
            title: "Backlog",
            tasks: [
                {
                    id:nanoid(),
                    title: "Create Landing page",
                    createdAt: new Date(),
                },
                {
                    id:nanoid(),
                    title: "Develop cool new feature",
                    createdAt: new Date(),
                },
                {
                    id:nanoid(),
                    title: "Fix page nav bug",
                    createdAt: new Date(),
                },
            ],
        },
        {
            id: nanoid(),
            title: "Ready for Dev",
            tasks: [],
        },
        {
            id: nanoid(),
            title: "In Progress",
            tasks: [],
        },
        {
            id: nanoid(),
            title: "QA",
            tasks: [],
        },
        {
            id: nanoid(),
            title: "Complete",
            tasks: [],
        },
    ]);

</script>

<template>
    <div>
        <draggableComponent
        v-model="columns"
        group="columns"
        item-key="id"
        class="flex gap-4 overflow-x-auto items-start">

            <template #item="">

            </template>

        </draggableComponent>
        <div 
        class="bg-gray-200 column p-5 rounded min-w-[250px]"
        v-for="column in columns" :key="column.id">
            <header class="font-bold mb-4">
                {{ column.title }}
            </header>
            <!-- Trello board task component -->
            <TrelloBoardTask v-for="task in column.tasks" :task="task" :key="task.id"
            >
            </TrelloBoardTask>
            <footer>
                    <button class="text-gray-500">+Add a card</button>
                </footer>
        </div>
    </div>
</template>