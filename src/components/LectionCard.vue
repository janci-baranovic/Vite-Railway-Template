<template>
	<div class="card">
		<div class="card-image">
			<figure class="image is-3by2">
				<img :src="'http://localhost:8000' + lection.image" alt="Placeholder image">
			</figure>
		</div>
		<div class="card-content">
			<div class="content">
				<h3 class="title is-5"> {{ this.lection.title }} </h3>
				<p v-html="this.lection.content.substring(0, 150) + '...'">
				</p>
			</div>
		</div>
		<footer class="card-footer">
			<router-link :to="{ name: 'Lection', params: { id: this.lection.id } }" class="card-footer-item has-text-primary">
				Začať lekciu
			</router-link>
			<router-link :to="{ name: 'LectionEdit', params: { id: this.lection.id } }" v-if="user && user.is_admin" class="card-footer-item">Upraviť</router-link>
			<a @click="isActive=true" v-if="user && user.is_admin" class="card-footer-item has-text-danger">
				Vymazať
			</a>
		</footer>
	</div>
	<modal :isActive="isActive" @closeModal="isActive=false" @delete="this.$emit('handleDelete', this.lection.id)"></modal>
</template>

<script>
import {mapState} from "pinia";
import {useAuthStore} from "../stores/auth.js";
import Modal from "./Modal.vue";
import axios from "axios";

export default {
	name: "LectionCard",

	props: {
		lection: Object
	},

	emits: ['handleDelete'],

	components: {
		Modal
	},

	data() {
		return {
			isActive: false,
		}
	},

	computed: {
		...mapState(useAuthStore, {user: "authUser"})
	},

}
</script>
