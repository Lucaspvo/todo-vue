<template>
    <div class="input-wrapper">
        <div class="extra-section">
            <slot></slot>
        </div>
        <input
            :value="modelValue"
            class="field"
            :type="inputType"
            :placeholder="inputPlaceholder"
            @input="handleInputChange"
            @keyup.enter="handleKeyupEnter"
            ref="input"
        />
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, Ref, ref } from 'vue';

interface Props {
    modelValue: string | undefined;
    type?: string;
    placeholder?: string;
    focus?: boolean;
}

export default defineComponent({
    name: 'TodoInput',
    props: {
        modelValue: {
            type: String || undefined,
            required: true,
        },
        type: {
            type: String,
            default: 'text',
        },
        placeholder: {
            type: String,
            default: '',  
        },
        focus: {
            type: Boolean,
            default: false,  
        },
    },
    emits: ['update:modelValue', 'keyup-enter'],
    setup(props: Props, { emit }) {
        const input: Ref<HTMLInputElement | null> = ref(null);
        function handleInputChange($event: Event): void {
            const target = $event.target as HTMLInputElement;
            emit('update:modelValue', target.value);
        }

        function handleKeyupEnter() {
            emit('keyup-enter');
        }

        onMounted(() => {
            if (props.focus) {
                input.value?.focus();
            }
        });
        
        return {
            inputType: props.type,
            inputPlaceholder: props.placeholder,
            input,
            handleInputChange,
            handleKeyupEnter,
        };
    },
});
</script>

<style scoped lang="sass">
.field
    width: 100%
    border: 0
    padding: 15px 30px 15px 10px
    border-radius: 0 3px 3px 0
    box-sizing: border-box
.field:focus
    outline: none
.extra-section
    display: flex
    background-color: white
    border-radius: 3px 0 0 3px
    padding-left: 10px
    justify-content: center
    align-items: center

.input-wrapper
    display: flex
</style>
