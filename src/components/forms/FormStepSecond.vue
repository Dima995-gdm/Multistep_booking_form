<template>
    <div class="step">
        <form>
            <Input label="Кол-во взрослых" v-model="adults" />
            <Input label="Кол-во детей" v-model="children" />
            <Input
                label="Дата заезда*"
                :errors="v$.dateArrival.$errors"
                placeholder="гггг.мм.дд"
                v-model="v$.dateArrival.$model"
                :imask="dateMask"
            />
            <Input
                label="Дата выезда*"
                :errors="v$.dateDeparture.$errors"
                placeholder="гггг.мм.дд"
                v-model="v$.dateDeparture.$model"
                :imask="dateMask"
            />
            <slot name="backStep"></slot>
            <slot name="nextStep"></slot>
        </form>
    </div>
</template>

<script>
import { useVuelidate } from '@vuelidate/core';
import { required, helpers } from '@vuelidate/validators';

import Input from '@/components/Input';

export default{
    components: { Input },
    data() {
        return {
            v$: useVuelidate(),
            adults: '2',
            children: '0',
            dateArrival: '',
            dateDeparture: '',
        }
    },
    validations () {
		return {
            dateArrival: { required: helpers.withMessage('Поле обязательное для заполнения', required) },
            dateDeparture: { 
                required: helpers.withMessage('Поле обязательное для заполнения', required),
                dateMoreArrival: helpers.withMessage('Дата выезда не должна быть раньше одного дня после заезда', 
                (value) => new Date(value).getTime() - new Date(this.form.dateArrival).getTime() >= 86400000)
            }
        }
    }
}
</script>
