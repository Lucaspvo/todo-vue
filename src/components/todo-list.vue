<template>
    <ul class="todo-list box-shadow">
        <li v-for="item in items" :key="item.id">
            <slot name="item" :item="item" :item-id="item.id"></slot>
        </li>
    </ul>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { TodoItem } from './todo-item.vue';

interface Props {
    listItems: TodoItem[] | never[],
}

export default defineComponent({
    name: 'TodoList',
    props: {
        listItems: {
            type: Array as PropType<TodoItem[] | never[]>,
            default: () => [],
        },
    },
    setup(props: Props) {
        return {
            items: props.listItems,
        };
    },
});
</script>

<style scoped>
.todo-list {
    background-color: white;
    border-radius: 3px;
    padding: 10px;
    list-style: none;
    font-family: 'Roboto', sans-serif;
    font-size: 13px;
    margin-bottom: 30px;
}
.todo-list li:not(:last-child) {
    border-bottom: 1px solid #eee;
}
</style>
