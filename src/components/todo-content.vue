<template>
    <div class="todo-content">
        <div class="image-background"></div>
        <div class="todo-wrapper">
            <h2 class="form-group todo-title">TODO</h2>
            <TodoCreator @create-todo="submitNewTodo" />
            <TodoList v-if="hasTodo" :listItems="todoList">
                <template v-slot:item="{ item, itemId }">
                    <TodoItem
                        :item="item"
                        @delete-todo="deleteTodo"
                        @update-todo="(data) => updateTodo(data, itemId)"
                    >
                        <template #complete-action="{ isEditable }">
                            <TodoCheckbox
                                v-model="item.completed"
                                @update="() => handleTodoCompletion(itemId)"
                                :disableCheckbox="isEditable"
                            />
                        </template>
                    </TodoItem>
                </template>
            </TodoList>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, Ref, computed, ComputedRef } from 'vue';
import TodoCheckbox from './core/checkbox.vue';
import TodoList from './todo-list.vue';
import TodoItem, { TodoItem as TodoItemInterface } from './todo-item.vue';
import TodoCreator from './todo-creator.vue';

export default defineComponent({
    name: 'TodoContent',
    components: {
        TodoCheckbox,
        TodoList,
        TodoItem,
        TodoCreator,
    },
    setup() {
        const todoCount: Ref<number> = ref(0);
        let todoList: TodoItemInterface[] = reactive([]);

        const hasTodo: ComputedRef<boolean> = computed(() => {
            return !!todoList.length;
        });

        function handleTodoCompletion(itemId: number) {
            const indexOf: number = todoList.findIndex(
                (item: TodoItemInterface) => item.id === itemId
            );
            const tempTodo = { ...todoList[indexOf], completed: !todoList[indexOf].completed };
            todoList.splice(indexOf, 1, tempTodo);
        }

        function submitNewTodo(todo: string) {
            todoList.push({
                id: todoCount.value,
                description: todo,
                completed: false,
            });
            todoCount.value++;
        }

        function updateTodo(todo: string, itemId: number) {
            const indexOf: number = todoList.findIndex(
                (item: TodoItemInterface) => item.id === itemId
            )
            todoList.splice(indexOf, 1, { ...todoList[indexOf], description: todo });
        }

        function deleteTodo(itemId: number) {
            const indexOf: number = todoList.findIndex(
                (item: TodoItemInterface) => item.id === itemId
            );
            todoList.splice(indexOf, 1);
        }

        return {
            todoList,
            hasTodo,
            handleTodoCompletion,
            submitNewTodo,
            deleteTodo,
            updateTodo,
        };
    }
});
</script>

<style scoped>
.todo-title{
    margin: 0;
    color: #F1EFF5;
    font-size: 33px;
    letter-spacing: 7px;
    font-family: sans-serif;
}
.todo-wrapper {
    width: 500px;
    margin-top: -200px;
}
.todo-content { 
    display: flex;
    flex-direction: column;
    align-items: center;
}
.image-background {
    background: linear-gradient(to right, rgba(255, 105, 180, 0.5), rgb(0,191,255, 0.5)), url('../assets/landscape wallpaper.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    height: 250px;
    background-position: center;
    width: 100%;
}
</style>