<template>
    <div class="container mt-5">
        <div class="col-sm-5 mx-auto">
			<div class="progress mb-4" style="height: 20px;">
				<div class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" :style="`width: ${indicator}%;`" aria-valuenow="20" aria-valuemin="0" aria-valuemax="100"></div>
			</div>
			<component :is="currentFormStep">
				<template v-slot:nextStep>
					<button type="submit" class="btn btn-primary" @click="nextStep">
						Продолжить
					</button>
				</template>
				<template v-slot:backStep>
					<button @click="backStep" type="button" class="btn btn-light me-3">
						Назад
					</button>
				</template>
			</component>
        </div>
    </div>
</template>

<script>

// import Input from '@/components/Input';
import FormStepFirst from '@/components/forms/FormStepFirst';
import FormStepSecond from '@/components/forms/FormStepSecond';
import FormStepThird from '@/components/forms/FormStepThird';
import FormResult from '@/components/result/Result';

export default {
	data() {
		return {
			formSteps: [
				FormStepFirst,
				FormStepSecond,
				FormStepThird,
				FormResult,
			],
			numberStep: 0,
			indicator: 0,
			form: {
				name: '',
				surname: '',
				email: '',
				phone: '',
				adults: '2',
				children: '0',
				dateArrival: '',
				dateDeparture: '',
				interests: [],
				comment: ''
			},
			dateMask: {
				mask: '0000.00.00',
                lazy: true
			}
		}
	},
	// validations () {
	// 	return {
	// 		form: {
	// 			name: { 
	// 				required: helpers.withMessage('Поле обязательное для заполнения', required),
	// 				containLetters: helpers.withMessage('Поле может содержать только буквы', (value) => /^[A-Za-zА-Яа-я]+$/.test(value))
	// 			},
	// 			surname: {
	// 				required: helpers.withMessage('Поле обязательное для заполнения', required),
	// 				containLetters: helpers.withMessage('Поле может содержать только буквы', (value) => /^[A-Za-zА-Яа-я]+$/.test(value))
	// 			},
	// 			email: { email: helpers.withMessage('Введите корректный email', email) },
	// 			phone: { 
	// 				required: helpers.withMessage('Поле обязательное для заполнения', required), 
	// 				numeric: helpers.withMessage('Поле может содержать только цифры', numeric), 
	// 			},
	// 			dateArrival: { required: helpers.withMessage('Поле обязательное для заполнения', required) },
	// 			dateDeparture: { 
	// 				required: helpers.withMessage('Поле обязательное для заполнения', required),
	// 				dateMoreArrival: helpers.withMessage('Дата выезда не должна быть раньше одного дня после заезда', 
	// 				(value) => new Date(value).getTime() - new Date(this.form.dateArrival).getTime() >= 86400000)
	// 			},
	// 			comment: { maxLengthValue: helpers.withMessage('Поле может содержать не более 250 символов', maxLength(250))}
	// 		}
			
	// 	}
	// },
	methods: {
		nextStep() {
			this.numberStep++
			this.indicator += 33.33		
		},
		backStep() {
			this.numberStep--	
			this.indicator -= 33.33	
		}

	},
	computed: {
		disableBtn1() {
			return this.v$.form.name.$invalid ||
				this.v$.form.surname.$invalid ||
				this.v$.form.email.$invalid ||
				this.v$.form.phone.$invalid
		},
		disableBtn2() {
			return this.v$.form.dateArrival.$invalid ||
				this.v$.form.dateDeparture.$invalid
		},
		disableBtn3() {
			return this.v$.form.comment.$invalid
		},
		currentFormStep() {
			return this.formSteps[this.numberStep]
		}
	}
};
</script>
