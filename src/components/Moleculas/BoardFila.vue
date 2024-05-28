<script>
    import Celda from '../Atomos/Celda.vue';

    export default {
        props: {
            fila: Array,
            jugadores: Array
        },
        components: {Celda},
        methods: {
            PosP1(fila, columna) {
                const P1 = this.jugadores[0];
                return P1.fila === fila && P1.columna === columna;
            },
            PosP2(fila, columna) {
                const P2 = this.jugadores[1];
                return P2.fila === fila && P2.columna === columna;
            },
            MnjClickCelda({fila, columna}) {
                this.$emit('CeldaClick', {fila, columna})
            },
        },
    };
</script>

<template>
    <div class="filaTablero">
        <Celda
        v-for="celda in fila"
        :key="celda.columna"
        :fila="celda.fila"
        :columna="celda.columna"
        :Jugador1="PosP1(celda.fila, celda.columna)"
        :Jugador2="PosP2(celda.fila, celda.columna)"
        :MuroHori="celda.MuroHori"
        :MuroVert="celda.MuroVert"
        
        @CeldaClick="MnjClickCelda"
        />
    </div>
</template>

<style scoped>
.filaTablero {
  display: flex;
}
</style>