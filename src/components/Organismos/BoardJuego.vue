<script>
import BoardFila from '../Moleculas/BoardFila.vue';
export default {
    props:{
        tablero: Array,
        jugadores: Array,
    },
    components: {BoardFila},
    methods: {
        MnjClickCelda({fila, columna}) {
                this.$emit('CeldaClick', {fila, columna})
            },
},
};
</script>

<template>
    <div class="Tablero">
      <BoardFila
        v-for="(fila, filaIndex) in tablero"
        :key="filaIndex"
        :fila="fila"
        :jugadores="jugadores"
        @CeldaClick="MnjClickCelda"/>
    </div>
  </template>

<style>
.Tablero{
    display: flex;
    flex-direction: column;
}
</style>