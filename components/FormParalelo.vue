<template>
    <div class="form-paralelo">
        <h1 class="form-paralelo__title">
            Calculo de resistencia equivalente en paralelo
        </h1>
        <UForm :validate="validate" :state="state" @submit="submit">
			<UFormGroup label="Valos de la resistencia" name="resistencia">
				<div class="form-serie__container">
					<UInput v-model="state.resistencia" placeholder="1" type="number" step="0.1"/>
					<USelect v-model="state.unidad" :options="unidades" />
					<UButton type="submit">+</UButton>
					<UButton @click="suma"> = </UButton>
				</div>
			</UFormGroup>
		</UForm>
        <div class="resultado">
			<span v-for="val in valores">({{ val }}Ω), </span>
		</div>
		<span v-if="resultado">={{ resultado }}</span>
    </div>
</template>

<script setup>
const unidades = ['mΩ','Ω','kΩ'];
const valores = ref([]);
const resultado = ref();
const suma = () => {
	const numerador = valores.value.reduce((a, b) => a * b, 1);
    let denominador = 0;
    for(let i = 0; i < valores.value.length; i++){
        denominador += valores.value.reduce((a, b, c) => {
            console.log(i,c, a * b)
            return c!=i ? a*b : a; 
        }, 1)
    }
    resultado.value = Number((numerador/denominador).toFixed(4));
}
const state = ref({
	resistencia: "1",
	unidad: 'Ω'
});
const validate = (state) => {
	const errors = [];
	if (!state.resistencia) errors.push({ path: "resistencia", message: "Ponle un valor" });
	return errors;
};
async function submit(event) {
	const { resistencia, unidad } = event.data;
	const calc = () => {
		const num = parseFloat(resistencia);
		if(unidad === 'mΩ') return num / 1000;
		if(unidad === 'kΩ') return num * 1000;
		return num;
	}
	valores.value.push(calc());
}
</script>

<style lang="scss">
    .form-paralelo {
        
        &__title {
            text-align: center;
        }
    }
</style>