<template>
    <div>
        <template v-if="!isEditable">
            <div
                class="todo-item"
                @mouseenter="handleMouseEnter"
                @mouseleave="handleMouseLeave"
            >
                <div class="complete-action-wrapper">
                    <slot name="complete-action" :isEditable="isEditable"></slot>
                </div>
                <p :class="{'todo-description': true, completed: item.completed}">
                    {{ item.description }}
                </p>
                <div
                    :class="{
                        'actions-wrapper': true,
                        'slide-left': isHovered === MOUSE_ENTER,
                        'slide-right': isHovered === MOUSE_LEAVE
                    }"
                >
                    <PencilIcon
                        v-if="!item.completed"
                        class="pencil-icon icon-s margin-right-s"
                        @click="editTodo"
                    />
                    <TrashIcon
                        class="trash-icon icon-s"
                        @click="() => deleteTodo(item.id)"
                    />
                </div>
            </div>
        </template>
        <template v-else>
            <TodoCreator
                class="todo-creator-item"
                :focus="true"
                :item="todo"
                :isEditable="true"
                v-model="todo"
                @cancel-editing="editTodo"
            />
        </template>
    </div>
</template>

<script lang="ts">
import {
    defineComponent,
    onMounted,
    PropType,
    ref,
    Ref,
    SetupContext,
    watch
} from 'vue';
import TodoCreator from './todo-creator.vue';
import { PencilIcon, TrashIcon } from "@heroicons/vue/24/outline";

export type TodoItem = {
    id: number,
    description: string,
    completed: boolean,
}

interface Props {
    item: TodoItem,
}

const MOUSE_ENTER: string = 'enter';
const MOUSE_LEAVE: string = 'leave';

export default defineComponent({
    name: 'TodoItem',
    components: {
        TodoCreator,
        PencilIcon,
        TrashIcon,
    },
    props: {
        item: {
            type: Object as PropType<TodoItem>,
            required: true,
        },
        modelValue: {
            type: String,
            default: '',
        },
    },
    setup(props: Props, context: SetupContext) {
        let isHovered: Ref<string> = ref('');
        let isEditable: Ref<boolean> = ref(false);
        let todo: Ref<string> = ref(props.item.description);

        function handleMouseEnter(): void {
            isHovered.value = MOUSE_ENTER;
        }

        function handleMouseLeave(): void {
            isHovered.value = MOUSE_LEAVE;
        }

        function deleteTodo(itemId: number) {
            context.emit('delete-todo', itemId);
        }

        function editTodo() {
            isHovered.value = '';
            isEditable.value = !isEditable.value;
        }

        onMounted(() => {
            watch(todo, () => {
                console.debug(todo.value);
                context.emit('update-todo', todo.value.trim());
                editTodo();
            });
        });

        return {
            isHovered,
            isEditable,
            todo,
            handleMouseEnter,
            handleMouseLeave,
            deleteTodo,
            editTodo,
            MOUSE_ENTER,
            MOUSE_LEAVE,
        };
    },
})
</script>

<style scoped>
.todo-item {
    position: relative;
    display: flex;
    padding: 10px 0;
    overflow: hidden;
}
.complete-action-wrapper {
    align-self: center;
    margin-right: 13px;
}
.todo-description {
    margin: 0;
    align-self: center;
    word-break: break-word;
}
.completed {
    text-decoration: line-through;
    color: darkgray;
}
.actions-wrapper {
    position: absolute;
    height: 100%;
    display: flex;
    align-items: center;
    right: 0;
    top: 50%;
    transform: translate(100%, -50%);
    padding: 3px 5px;
    background-color: white;
    box-shadow: -10px 0px 5px white;
}
</style>

<style lang="sass">
.todo-creator-item
    margin: 0 -10px 0 -10px !important
    
    .input-wrapper
        box-shadow: none !important
        margin: 0 !important
</style>
