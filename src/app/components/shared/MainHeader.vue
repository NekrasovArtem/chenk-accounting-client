<script setup>

import {useBaseStore} from "@/app/stores/base.js";
import router from "@/app/router/index.js";
import {useToastStore} from "@/app/stores/toast.js";

const {deleteToken} = useBaseStore()
const {successMessage} = useToastStore()

function logout() {
	deleteToken()
	successMessage('Выход из аккаунта')
	router.replace({name: 'Auth'})
}
</script>

<template>
	<header class="header">
		<div class="header__wrapper">
			<router-link :to="{ name: 'Home' }" class="header__logo">
				<img class="header__logo-image" src="/img/logo.png" alt="alt"/>
			</router-link>
			<nav class="header__nav">
				<router-link :to="{ name: 'Events' }" class="link header__nav-item">Мероприятия</router-link>
				<router-link :to="{ name: 'Groups' }" class="link header__nav-item">Группы</router-link>
				<router-link :to="{ name: 'About' }" class="link header__nav-item">О системе</router-link>
				<a class="link header__nav-item" @click="logout">Выйти</a>
			</nav>
		</div>
	</header>
</template>
