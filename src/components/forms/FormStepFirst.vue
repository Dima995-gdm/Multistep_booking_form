<template>
    <div class="step">
        <form novalidate>
            <Input
                label="Имя*"
                :errors="v$.name.$errors"
                v-model="v$.name.$model"
            />
            <Input
                label="Фамилия*"
                :errors="v$.surname.$errors"
                v-model="v$.surname.$model"
            />
            <Input
                label="Эл. почта"
                :errors="v$.email.$errors"
                placeholder="mail@mail.ru"
                v-model="v$.email.$model"
            />
            <Input
                label="Телефон*"
                :errors="v$.phone.$errors"
                placeholder="79999999999"
                v-model="v$.phone.$model"
                :imask="phoneMask"
            />
            <slot name="nextStep"></slot>
        </form>
    </div>
</template>

<script>
import { useVuelidate } from '@vuelidate/core';
import { required, numeric, email, helpers } from '@vuelidate/validators';

import Input from '@/components/Input';

export default{
    components: { Input },
    data() {
        return {
            v$: useVuelidate(),
            name: '',
            surname: '',
            email: '',
            phone: '',
            phoneMask: {
                mask: '70000000000',
                lazy: true
            },
        }
    },
    validations () {
		return {
            name: { 
                required: helpers.withMessage('Поле обязательное для заполнения', required),
                containLetters: helpers.withMessage('Поле может содержать только буквы', (value) => /^[A-Za-zА-Яа-я]+$/.test(value))
            },
            surname: {
                required: helpers.withMessage('Поле обязательное для заполнения', required),
                containLetters: helpers.withMessage('Поле может содержать только буквы', (value) => /^[A-Za-zА-Яа-я]+$/.test(value))
            },
            email: { email: helpers.withMessage('Введите корректный email', email) },
            phone: { 
                required: helpers.withMessage('Поле обязательное для заполнения', required), 
                numeric: helpers.withMessage('Поле может содержать только цифры', numeric), 
            },
		}
	}
};
</script>
