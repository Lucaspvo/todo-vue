<template>
    <div class="create-todo-section">
        <TodoInput
            class="form-group box-shadow"
            v-model="todo"
            :focus="focus"
            @keyup-enter="submitNewTodo"
        >
            <TodoCheckbox :disableCheckbox="true" />
        </TodoInput>
        <PaperAirplaneIcon
            v-if="!!todo"
            :class="{
                'icon-s': true,
                'add-todo': !!todo,
            }"
            @click="submitNewTodo"
        />
        <XMarkIcon
            v-if="isEditable"
            :class="{
                'icon-s': true,
                'cancel-editing': !todo,
                'cancel-editing-right': !!todo
            }"
            @click="cancelEditing"
        />
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref, SetupContext } from 'vue';
import TodoInput from './core/input.vue';
import { PaperAirplaneIcon, XMarkIcon } from "@heroicons/vue/24/outline";
import TodoCheckbox from './core/checkbox.vue';

interface Props {
    focus: boolean;
    isEditable: boolean;
    modelValue: string;
}

export default defineComponent({
    name: 'TodoCreator',
    components: {
        TodoInput,
        PaperAirplaneIcon,
        TodoCheckbox,
        XMarkIcon,
    },
    props: {
        modelValue: {
            type: String,
            default: '',
        },
        focus: {
            type: Boolean,
            default: false,  
        },
        isEditable: {
            type: Boolean,
            default: false,  
        },
    },
    setup(props: Props, context: SetupContext) {
        const todo: Ref<string> = ref(props.modelValue);

        function submitNewTodo() {
            if (todo.value) {
                context.emit('create-todo', todo.value);
                context.emit('update:modelValue', todo.value);
                todo.value = '';
            }
        }

        function cancelEditing() {
            context.emit('cancel-editing');
        }

        return {
            todo,
            submitNewTodo,
            cancelEditing,
        };
    },
})
</script>

<style scoped>
.create-todo-section {
    position: relative;
}
.add-todo, .cancel-editing, .cancel-editing-right {
    position: absolute;
    top: 50%;
    right: 0;
    margin-left: 10px;
    padding: 5px;
    background-color: white;
    border-radius: 50px;
    cursor: pointer;
    animation: rotate 0.8s ease;
    animation-fill-mode: forwards;
    transform-origin: center center;
}
.add-todo:hover, .cancel-editing:hover, .cancel-editing-right:hover {
    background-color: black;
    color: white;
}

.cancel-editing-right {
    right: -40px;
}
</style>
