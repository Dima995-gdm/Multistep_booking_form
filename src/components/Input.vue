<template>
    <div class="mb-3">
        <label class="form-label">{{ label }}</label>
        <input
            :class="['form-control', { 'is-invalid': errors.length }]"
            :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)"
            :placeholder="placeholder"
            v-imask="imask"
        />
        <div v-if="errors.length" class="invalid-feedback">
            {{ errors[0].$message }}
        </div>
    </div>
</template>

<script>
import { IMaskDirective } from 'vue-imask';

export default {
    props: {
        label: {
            required: true,
            type: String
        },
        errors: {
            required: false,
            type: Array,
            default() {
                return []
            }
        },
        placeholder: {
            required: false,
            type: String
        },
        ['modelValue']: {
            required: true,
        },
        imask: {
            required: false,
            type: Object
        }
    },
    emits: ['update:modelValue'],
    directives: {
        imask: IMaskDirective
    }
}

</script>