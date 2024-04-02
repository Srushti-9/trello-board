<script setup lang="ts">
    import { nanoid } from "nanoid";
    import draggableComponent from "vuedraggable";
    import {useLocalStorage} from "@vueuse/core";
    import { Column, Task} from '~/types';
    import TrelloBoardTask from "./TrelloBoardTask.vue";
    import DragHandle from "./DragHandle.vue";
    import NewTask from "./NewTask.vue";
    import { nextTick } from 'vue'

    import { useKeyModifier } from "@vueuse/core";
    const columns = useLocalStorage<Column[]>("trelloboard", [
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
    ],
    {
    serializer: {
      read: (value) => {
        return JSON.parse(value).map((column: Column) => {
          column.tasks = column.tasks.map((task: Task) => {
            task.createdAt = new Date(task.createdAt);
            return task;
          });
          return column;
        });
      },
      write: (value) => JSON.stringify(value),
    },
  });

    const alt = useKeyModifier("Alt")

    function createColumn(){
        const column: Column = {
            id:nanoid(),
            title: "",
            tasks: [],
        }

        columns.value.push(column);

        nextTick(() => {
            (document.querySelector(".column:last-of-type .title-input") as HTMLInputElement).focus()
        });
        
    }

</script>

<template>
    <div class="flex items-start overflow-x-auto gap-4">
        <draggableComponent
        v-model="columns"
        group="columns"
        item-key="id"
        :animation="150"
        handle=".drag-handle"
        class="flex gap-4 items-start">

            <template #item="{element: column}: {element: Column}">
                <div 
                class="bg-gray-200 column p-5 rounded min-w-[250px]">
                    <header class="font-bold mb-4">
                        <DragHandle/>
                        <input
                            class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
                            @keyup.enter="($event.target as HTMLInputElement).blur()"
                            @keydown.backspace="
                                column.title === ''
                                ? (columns = columns.filter((c) => c.id !== column.id))
                                : null
                            "
                            type="text"
                            v-model="column.title"
                        />
                        <!-- {{ column.title }} -->
                    </header>
                    <draggableComponent
                        v-model="column.tasks"
                        :group="{name: 'tasks', pull: alt ? 'clone' : true}"
                        item-key="id"
                        :animation="150"
                        handle=".drag-handle">

                        <template #item="{element: task}: {element: Task}">
                            <div>
                                <TrelloBoardTask :task="task" @delete="column.tasks = column.tasks.filter((t) => t.id !== $event)"/>
                            </div>
                        </template>
                   
                    </draggableComponent>
                    
                    
                    <footer>
                        <!-- <button class="text-gray-500">+Add a card</button> -->
                        <new-task @add="column.tasks.push($event)"/>
                    </footer>
                </div>
            </template>
        </draggableComponent>
        <!-- <pre>
            {{ columns }}
        </pre> -->
        <button 
        @click="createColumn"
        class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50">
            + Add Another Column
        </button>
    </div>
</template>