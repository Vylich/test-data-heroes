<script setup>
import { ref } from 'vue'

defineProps({
	character: Object,
})

const count = ref(0)
</script>

<template>
	<div class="card">
		<img :src="character.image" />
		<div class="info">
			<div class="header">
				<h2>{{ character.name }}</h2>
				<div class="status">
					<span v-if="character.status === 'Alive'" class="indicator"></span>
					<span
						v-else-if="character.status === 'Dead'"
						class="indicator indicator--dead"
					></span>
					<span v-else class="indicator indicator--unknown"></span>
					<p>{{ character.status }}</p>
					<span> - </span>
					<p>{{ character.species }}</p>
				</div>
			</div>
			<div class="block">
				<span class="key">Last known location:</span>
				<span class="value">{{ character.location.name }}</span>
			</div>
			<div class="block">
				<span class="key">First seen in:</span>
				<span class="value">{{ character.origin.name }}</span>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.card {
	width: 600px;
	height: 220px;
	background-color: rgb(60, 62, 68);
	border-radius: 0.5rem;
	box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
		rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
	display: flex;
	padding: 0;
	overflow: hidden;

	h2 {
		margin: 0;
	}

	p {
		margin: 0;
	}
}

img {
	width: 220px;
	height: 220px;
	object-fit: cover;
}

.info {
	padding: 1rem;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

.header {
	display: flex;
	flex-direction: column;
	align-items: start;

	h2 {
		font-size: 1.25rem;
	}
}

.status {
	display: flex;
	align-items: center;
}

.block {
	display: flex;
	flex-direction: column;
	align-items: start;
}

.indicator {
	width: 0.5rem;
	height: 0.5rem;
	background-color: rgb(85, 204, 68);
	border-radius: 50%;
	margin-right: 0.5rem;
}

.indicator--dead {
	background-color: rgb(214, 61, 46);
}

.indicator--unknown {
	background-color: rgb(128, 128, 128);
}

.key {
	color: rgb(158, 158, 158);
}
</style>
