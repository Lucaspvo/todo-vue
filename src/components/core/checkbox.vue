<template>
    <div class="checkbox-input">
        <input
            :disabled="checkboxDisabled"
            type="checkbox"
            v-model="checked"
        />
        <span
            @click="handleCompletionClick"
            :class="{checkbox: !checkboxDisabled, 'disabled-checkbox': checkboxDisabled}"
        ></span>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref } from 'vue';

interface Props {
    modelValue: boolean,
    disableCheckbox: boolean,
}

export default defineComponent({
    name: 'TodoCheckbox',
    props: {
        modelValue: {
            type: Boolean,
            default: false,
        },
        disableCheckbox: {
            type: Boolean,
            default: false,
        },
    },
    setup(props: Props, { emit }) {
        let checked: Ref<boolean> = ref(props.modelValue || false);

        function handleCompletionClick(): void {
            checked.value = !checked.value;
            emit('update', checked.value);
        }
        
        return {
            checkboxDisabled: props.disableCheckbox || false,
            checked,
            handleCompletionClick,
        };
    },
});
</script>

<style scoped>
.checkbox-input > input {
    display: none;
}

.checkbox-input input:checked ~ .checkbox::after {
    content: "\2713";
    color: white;
}
.checkbox-input input:checked ~ .checkbox {
    background-color: #19A7CE;
}

.checkbox:hover {
    background-color: #19A7CE;
}
.checkbox:hover::after {
    content: "\2713";
    color: white;
}

.checkbox, .disabled-checkbox {
    display: block;
    position: relative;
    border: 1px solid #AFD3E2;
    border-radius: 30px;
    width: 25px;
    height: 25px;
    cursor: pointer;
}
.checkbox::after, .disabled-checkbox::after {
    position: absolute;
    content: "";
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 20px;
    font-weight: 900; 
}
.disabled-checkbox {
    cursor: default;
}
</style>
