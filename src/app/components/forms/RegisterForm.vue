<script setup>
import InputText from "@/app/components/inputs/InputText.vue";
import InputPassword from "@/app/components/inputs/InputPassword.vue";
import {reactive} from "vue";
import {registration} from "@/app/api/index.js";
import {useToastStore} from "@/app/stores/toast.js";
import router from "@/app/router/index.js";

const {successMessage, errorMessage} = useToastStore()

const formData = reactive({
	name: "",
	surname: "",
	patronymic: "",
	phone: "",
	email: "",
	password: "",
});

async function onSubmit() {
	const promise = registration(formData)

	if (promise.status === 200) {
		successMessage('Вы успешно зарегистрировались')
		await router.replace({name: 'Auth'})
	} else {
		errorMessage('Ошибка!')
	}
}
</script>

<template>
	<form class="form" @submit.prevent="onSubmit">
		<h1 class="form__title">Регистрация</h1>
		<div class="form__items">
			<div class="form__item">
				<InputText
					label="Имя"
					placeholder="Имя"
					v-model="formData.name"
					required
				/>
			</div>
			<div class="form__item">
				<InputText
					label="Фамилия"
					placeholder="Фамилия"
					v-model="formData.surname"
					required
				/>
			</div>
			<div class="form__item form__item--full">
				<InputText
					label="Отчество"
					placeholder="Отчество"
					v-model="formData.patronymic"
				/>
			</div>
			<div class="form__item">
				<InputText
					label="Телефон"
					placeholder="+7"
					v-model="formData.phone"
					required
				/>
			</div>
			<div class="form__item">
				<InputText
					label="E-mail"
					placeholder="E-mail"
					v-model="formData.email"
					required
				/>
			</div>
			<div class="form__item form__item--full">
				<InputPassword
					label="Пароль"
					placeholder="Пароль"
					v-model="formData.password"
					required
				/>
			</div>
			<div class="form__item form__item--full">
				<InputPassword
					label="Пароль"
					placeholder="Пароль"
					v-model="formData.passwordRepeat"
					required
				/>
			</div>
		</div>
		<div class="form__bottom">
			<button type="submit" class="btn">
				<span class="btn__text">Зарегистрироваться</span>
			</button>
			<router-link :to="{ name: 'Auth' }" class="link">Войти</router-link>
		</div>
	</form>
</template>
