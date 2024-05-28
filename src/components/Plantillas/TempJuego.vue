<script>
import TurnoMsg from '../Organismos/TurnoMsg.vue';
import BoardJuego from '../Organismos/BoardJuego.vue';
import Ctrls from '../Moleculas/Ctrls.vue';

export default {
    components: { TurnoMsg, BoardJuego, Ctrls },
    data() {
        return {
            tablero: this.IniciarTablero(),
            jugadores: [
                { id: 1, fila: 0, columna: 4, Muros: 10, MetaFila: 8 },
                { id: 2, fila: 8, columna: 4, Muros: 10, MetaFila: 0 }
            ],
            JugActual: 0,
            Ganador: null,
            Turnomsg: '',
            Accion: 'mover',
            OrientacionMuro: 'horizontal',
            LugarMuro: false,
        };
    },
    computed: {
        Muros() {
            return this.jugadores[this.JugActual].Muros;
        },
    },
    mounted() {
        this.$el.focus();
    },
    methods: {
        IniciarTablero() {
            const tablero = [];
            for (let i = 0; i < 9; i++) {
                const fila = [];
                for (let j = 0; j < 9; j++) {
                    fila.push({ fila: i, columna: j, MuroHori: false, MuroVert: false });
                }
                tablero.push(fila);
            }
            return tablero;
        },
        MnjMov(direccion) {
            if (this.Accion != 'mover') return;

            const jugador = this.jugadores[this.JugActual];
            let nuevaFila = jugador.fila;
            let nuevaCol = jugador.columna;

            switch (direccion) {
                case 'up':
                    nuevaFila -= 1;
                    break;
                case 'down':
                    nuevaFila += 1;
                    break;
                case 'left':
                    nuevaCol -= 1;
                    break;
                case 'right':
                    nuevaCol += 1;
                    break;
            }
            if (this.MovValido(jugador, nuevaFila, nuevaCol)) {
                jugador.fila = nuevaFila;
                jugador.columna = nuevaCol;
                if (this.VerifGanador(jugador)) {
                    this.Ganador = jugador.id;
                    this.Turnomsg = `Jugador ${jugador.id} ha ganado!`;
                    alert (`Jugador ${jugador.id} ha ganado!`)
                    this.tablero = this.IniciarTablero();
            this.jugadores = [
                { id: 1, fila: 0, columna: 4, Muros: 10, MetaFila: 8 },
                { id: 2, fila: 8, columna: 4, Muros: 10, MetaFila: 0 }
            ];
            this.JugActual = 0;
            this.Ganador = null;
            this.Turnomsg = 'Turno del jugador 1';
            this.Accion = 'mover';
            this.OrientacionMuro = 'horizontal';
                } else {
                    this.cambiarJug();
                }
            } else {
                alert('Movimiento no válido');
            }
        },
        MnjMuroPos() {
            this.Accion = 'muro';
        },
        MovValido(jugador, fila, columna) {
            const filaDif = Math.abs(jugador.fila - fila);
            const ColDif = Math.abs(jugador.columna - columna);
            if (filaDif + ColDif !== 1) return false;
            for (const otroJug of this.jugadores) {
                if (otroJug !== jugador && otroJug.fila === fila && otroJug.columna === columna) {
                    return false;
                }
            }
            if (filaDif === 1) {
                if (jugador.fila < fila && 
                this.tablero[jugador.fila][jugador.columna].MuroHori) return false;
                if (jugador.fila > fila && 
                this.tablero[fila][columna].MuroHori) return false;
            }
            if (ColDif === 1) {
                if (jugador.columna < columna && 
                this.tablero[jugador.fila][jugador.columna].MuroVert) return false;
                if (jugador.columna > columna && 
                this.tablero[fila][columna].MuroVert) return false;
            }
            return fila >= 0 && fila < 9 && columna >= 0 && columna < 9;
        },

        Flechas(event) {
            const MapaKey = {
                ArrowUp: 'up',
                ArrowDown: 'down',
                ArrowLeft: 'left',
                ArrowRight: 'right',
            };
            const direccion = MapaKey[event.key];
            if (direccion && this.Accion === 'mover') {
                this.MnjMov(direccion);
            }
        },
        VerifGanador(jugador) {
            return jugador.fila === jugador.MetaFila;
        },
        cambiarJug() {
            this.JugActual = this.JugActual === 0 ? 1 : 0;
            this.Turnomsg = `Turno del jugador ${this.JugActual + 1}`;
            this.Accion = 'mover';
        },
        reiniciarJuego() {
            this.tablero = this.IniciarTablero();
            this.jugadores = [
                { id: 1, fila: 0, columna: 4, Muros: 10, MetaFila: 8 },
                { id: 2, fila: 8, columna: 4, Muros: 10, MetaFila: 0 }
            ];
            this.JugActual = 0;
            this.Ganador = null;
            this.Turnomsg = 'Turno del jugador 1';
            this.Accion = 'mover';
            this.OrientacionMuro = 'horizontal';
        },
        MuroValido(fila, columna, orientacion) {
            if (orientacion === 'horizontal') {
                if (
                    columna >= 8 ||
                    this.tablero[fila][columna].MuroHori || this.tablero[fila][columna + 1].MuroHori
                ) {
                    return false;
                }
                if (
                    (fila > 0 && this.tablero[fila - 1][columna].MuroVert && 
                    this.tablero[fila - 1][columna + 1].MuroVert) ||
                    (fila < 8 && this.tablero[fila + 1][columna].MuroVert && 
                    this.tablero[fila + 1][columna + 1].MuroVert)
                ) {
                    return false;
                }
            } else {
                if (
                    fila >= 8 ||
                    this.tablero[fila][columna].MuroVert || this.tablero[fila + 1][columna].MuroVert
                ) {
                    return false;
                }
                if (
                    (columna > 0 && this.tablero[fila][columna - 1].MuroHori && 
                    this.tablero[fila + 1][columna - 1].MuroHori) ||
                    (columna < 8 && this.tablero[fila][columna + 1].MuroHori && 
                    this.tablero[fila + 1][columna + 1].MuroHori)
                ) {
                    return false;
                }
            }
            return true;
        },
        CaminoDisponible(jugador) {
            const direcciones = [
                { fila: -1, columna: 0 },
                { fila: 1, columna: 0 },
                { fila: 0, columna: -1 },
                { fila: 0, columna: 1 },
            ];

            const visitado = Array.from({ length: 9 }, () => Array(9).fill(false));
            const cola = [{ fila: jugador.fila, columna: jugador.columna }];
            visitado[jugador.fila][jugador.columna] = true;

            while (cola.length > 0) {
                const { fila, columna } = cola.shift();
                if (fila === jugador.MetaFila) return true;

                for (const direccion of direcciones) {
                    const nuevaFila = fila + direccion.fila;
                    const nuevaCol = columna + direccion.columna;

                    if (
                        nuevaFila >= 0 && nuevaFila < 9 && 
                        nuevaCol >= 0 && nuevaCol < 9 &&
                        !visitado[nuevaFila][nuevaCol] && 
                        this.MovValido({ fila, columna }, nuevaFila, nuevaCol)
                    ) {
                        visitado[nuevaFila][nuevaCol] = true;
                        cola.push({ fila: nuevaFila, columna: nuevaCol });
                    }
                }
            }
            return false;
        },
        MnjPonerMuro(fila, columna) {
            const jugador = this.jugadores[this.JugActual];
            if (jugador.Muros <= 0) {
                alert('Te quedaste sun muros jugador '+ (this.JugActual+1));
                return;
            }
            if (this.MuroValido(fila, columna, this.OrientacionMuro)) {
                if (this.OrientacionMuro === 'horizontal') {
                    this.tablero[fila][columna].MuroHori = true;
                    this.tablero[fila][columna + 1].MuroHori = true;
                } else {
                    this.tablero[fila][columna].MuroVert = true;
                    this.tablero[fila + 1][columna].MuroVert = true;
                }

                if (this.CaminoDisponible(this.jugadores[0]) && this.CaminoDisponible(this.jugadores[1])) {
                    jugador.Muros -= 1;
                    this.cambiarJug();
                    this.LugarMuro = false;
                } else {
                    if (this.OrientacionMuro === 'horizontal') {
                        this.tablero[fila][columna].MuroHori = false;
                        this.tablero[fila][columna + 1].MuroHori = false;
                    } else {
                        this.tablero[fila][columna].MuroVert = false;
                        this.tablero[fila + 1][columna].MuroVert = false;
                    }
                    alert('No puedes bloquear por completo el camino de tu rival');
                }

            } else {
                alert('No puedes colocar el muro en ese lugar');
            }
        },
        MnjClickCelda({ fila, columna }) {
            if (this.Accion === 'muro') {
                this.MnjPonerMuro(fila, columna);
            }
        },
        cambiarAccion() {
            this.Accion = this.Accion === 'mover' ? 'muro' : 'mover';
        },
        setOrientacionMuro(orientacion) {
            this.OrientacionMuro = orientacion;
        },
    },
};
</script>

<template>
    <div class="TempJ" @keydown="Flechas" tabindex="0">
        <TurnoMsg :msg="Turnomsg" />
        <BoardJuego :tablero="tablero" :jugadores="jugadores" 
        @CeldaClick="MnjClickCelda" />
        <Ctrls :Muros="Muros" :Accion="Accion" 
        @mover="MnjMov" @PonerMuro="MnjMuroPos" 
        @reiniciar="reiniciarJuego" @ActivAccion="cambiarAccion" 
        @setOrientacionMuro="setOrientacionMuro" />
    </div>
</template>

<style scoped>
.TempJ {
    display: flex;
    flex-direction: column;
    align-items: center;
}
</style>
