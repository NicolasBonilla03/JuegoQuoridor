<script>
export default{
    props:{
        Muros: Number,
        Accion: String
    }
};
</script>

<template>
    <section class="Ctrls">
        <button @click="$emit('reiniciar')">Reiniciar</button>
        <button @click="$emit('ActivAccion')">{{ Accion === 'mover' ? 'Cambiar a Poner  Muro' : 'Cambiar a Moverse'}}</button>
        <div v-if="Accion === 'mover'"></div>
        <div v-if="Accion === 'muro'">
            <p>Selecciona la orientacion del muro:</p>
            <button @click="$emit('setOrientacionMuro', 'horizontal')">Horizontal</button>
            <button @click="$emit('setOrientacionMuro', 'vertical')">Vertical</button>
            <p>Muros restantes: {{ Muros }}</p>
        </div>
    </section>
</template>

<style scoped>
.Ctrls {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>