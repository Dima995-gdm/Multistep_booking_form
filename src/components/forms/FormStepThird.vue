<template>
    <div class="step">
        <form>
            <div class="mb-3">Выберите интересы:</div>
            <div class="mb-3" v-for="interest, index in listInterests" :key="index">
                <input class="form-check-input me-2" type="checkbox" :value="interest" v-model="interests">
                <label class="form-check-label">{{ interest }}</label>
            </div>
            <div class="mb-3">
                <label class="form-label">Комментарий</label>
                <textarea
                rows="7"
                :class="['form-control', { 'is-invalid': v$.comment.$errors.length }]"
                v-model="v$.comment.$model"
                />
                <div v-if="v$.comment.$errors.length" class="invalid-feedback">
                    {{ v$.comment.$errors[0].$message }}
                </div>
            </div>
            <slot name="backStep"></slot>
            <slot name="nextStep"></slot>
        </form>
	</div>
</template>

<script>
import { useVuelidate } from '@vuelidate/core';
import { helpers, maxLength } from '@vuelidate/validators';

export default {
    data() {
        return {
            v$: useVuelidate(),
            interests: [],
			comment: '',
            listInterests: [
				'Индивидуальные экскурсии',
				'Обмен валюты',
				'Персональный переводчик',
				'Заказ такси',
				'Сувенирная продукция',
			],
            dateMask: {
				mask: '0000.00.00',
                lazy: true
			}
        }
    },
    validations() {
        return {
            comment: { maxLengthValue: helpers.withMessage('Поле может содержать не более 250 символов', maxLength(250))}
        }
    }
}

</script>