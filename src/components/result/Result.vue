<template>
    <div class="step">
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
</template>