<template>
    <div class="container mt-5">
        <div class="col-sm-5 mx-auto">
			<div class="progress mb-4" style="height: 20px;">
				<div class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" :style="`width: ${indicator}%;`" aria-valuenow="20" aria-valuemin="0" aria-valuemax="100"></div>
			</div>
			<div class="step" v-show="currentStep === 1">
				<form novalidate>
					<Input 
					label="Имя*"
					:errors="v$.form.name.$errors"
					v-model="v$.form.name.$model" 
					/>
					<Input 
					label="Фамилия*"
					:errors="v$.form.surname.$errors"
					v-model="v$.form.surname.$model" 
					/>
					<Input 
					label="Эл. почта"
					:errors="v$.form.email.$errors"
					placeholder="mail@mail.ru"
					v-model="v$.form.email.$model" 
					/>
					<Input 
					label="Телефон*"
					:errors="v$.form.phone.$errors"
					placeholder="79999999999"
					v-model="v$.form.phone.$model" 
					:imask="phoneMask"
					/>
					<button type="button" :disabled="disableBtn1" class="btn btn-primary" @click="nextStep">Продолжить</button>
				</form>
			</div>
			<div class="step" v-show="currentStep === 2">
				<form>
					<Input 
					label="Кол-во взрослых"
					v-model="form.adults" 
					/>
					<Input 
					label="Кол-во детей"
					v-model="form.children" 
					/>
					<Input 
					label="Дата заезда*"
					:errors="v$.form.dateArrival.$errors"
					placeholder="гггг.мм.дд"
					v-model="v$.form.dateArrival.$model" 
					:imask="dateMask"
					/>
					<Input 
					label="Дата выезда*"
					:errors="v$.form.dateDeparture.$errors"
					placeholder="гггг.мм.дд"
					v-model="v$.form.dateDeparture.$model"
					:imask="dateMask" 
					/>
					<button @click="backStep" type="button" class="btn btn-light me-3">Назад</button>
					<button type="button" :disabled="disableBtn2" class="btn btn-primary" @click="nextStep">Продолжить</button>
				</form>
			</div>
			<div class="step" v-show="currentStep === 3">
				<form>
					<div class="mb-3">Выберите интересы:</div>
					<div class="mb-3" v-for="interest, index in listInterests" :key="index">
						<input class="form-check-input me-2" type="checkbox" :value="interest" v-model="form.interests">
						<label class="form-check-label">{{ interest }}</label>
					</div>
					<div class="mb-3">
						<label class="form-label">Комментарий</label>
						<textarea
						rows="7"
						:class="['form-control', { 'is-invalid': v$.form.comment.$errors.length }]"
						v-model="v$.form.comment.$model"
						/>
						<div v-if="v$.form.comment.$errors.length" class="invalid-feedback">
							{{ v$.form.comment.$errors[0].$message }}
						</div>
					</div>
					<button @click="backStep" type="button" class="btn btn-light me-3">Назад</button>
					<button type="button" :disabled="disableBtn3" class="btn btn-primary" @click="nextStep">Продолжить</button>
				</form>
			</div>
			<div class="step" v-show="currentStep === 4">
				<div class="result mb-3 border p-3 rounded">
					<h1 class="text-center mb-4">Данные</h1>
					<ul class="result__wrap list-group list-group-flush mb-4">
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Имя:</div>
							<div class="result__item-value">{{ form.name }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Фамилия:</div>
							<div class="result__item-value">{{ form.surname }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Эл.почта:</div>
							<div class="result__item-value">{{ form.email }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Телефон:</div>
							<div class="result__item-value">{{ form.phone }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Кол-во взрослых:</div>
							<div class="result__item-value">{{ form.adults }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Кол-во детей:</div>
							<div class="result__item-value">{{ form.children }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Дата заезда:</div>
							<div class="result__item-value">{{ form.dateArrival }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Дата выезда:</div>
							<div class="result__item-value">{{ form.dateDeparture }}</div>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Интересы:</div>
							<ul v-for="interest, index in form.interests" :key="index">
								<li>{{ interest }}</li>
							</ul>
						</li>
						<li class="result__item list-group-item">
							<div class="result__item-name mb-2 fw-bold">Комментарий:</div>
							<div class="result__item-value text-break">{{ form.comment }}</div>
						</li>
					</ul>
					<button @click="backStep" type="button" class="btn btn-light mb-2 d-block mx-auto">Назад</button>
					<button type="button" class="btn btn-success d-block mx-auto">Оформить бронирование</button>
				</div>
			</div>
        </div>
    </div>
</template>

<script>
import { useVuelidate } from '@vuelidate/core';
import { required, numeric, email, maxLength, helpers } from '@vuelidate/validators';


import Input from '@/components/Input';

export default {
	components: { Input },
	data() {
		return {
			v$: useVuelidate(),
			currentStep: 1,
			listInterests: [
				'Индивидуальные экскурсии',
				'Обмен валюты',
				'Персональный переводчик',
				'Заказ такси',
				'Сувенирная продукция',
			],
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
			phoneMask: {
                mask: '70000000000',
                lazy: true
            },
			dateMask: {
				mask: '0000.00.00',
                lazy: true
			}
		}
	},
	validations () {
		return {
			form: {
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
				dateArrival: { required: helpers.withMessage('Поле обязательное для заполнения', required) },
				dateDeparture: { 
					required: helpers.withMessage('Поле обязательное для заполнения', required),
					dateMoreArrival: helpers.withMessage('Дата выезда не должна быть раньше одного дня после заезда', 
					(value) => new Date(value).getTime() - new Date(this.form.dateArrival).getTime() >= 86400000)
				},
				comment: { maxLengthValue: helpers.withMessage('Поле может содержать не более 250 символов', maxLength(250))}
			}
			
		}
	},
	methods: {
		nextStep() {
			this.currentStep++
			this.indicator += 33.33		
		},
		backStep() {
			this.currentStep--	
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
		}
	}
};
</script>
