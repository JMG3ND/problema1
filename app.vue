<template>
	<div class="screen">
		<div class="screen__container">
			<UTabs :items="items">
				<template #item="{ item }">
					<div v-if="item.key === 'serie'">
						<FormSerie/>
					</div>
					<div v-if="item.key === 'paralelo'">
						<FormParalelo/>
					</div>
				</template>
			</UTabs>
		</div>
	</div>
</template>

<script setup>
const items = [{
	key: 'serie',
	label: 'Serie',
	description: 'Calculadora de resistencia equivalente de circuito en serie'
}, {
	key: 'paralelo',
	label: 'Paralelo',
	description: 'Calculadora de resistencia equivalente de circuito en paralelo'
}]
</script>

<style lang="scss">
@import url("@/assets/default_styles.scss");

.screen {
	height: 90vh;
	display: flex;
	align-items: center;
	justify-content: center;

	&__container {
		width: 50%;
		max-width: 800px;
	}
}
</style>
