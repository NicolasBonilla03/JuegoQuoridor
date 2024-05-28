<script>
export default{
    props: {
        fila: Number,
        columna: Number,
        Jugador1: Boolean,
        Jugador2: Boolean,
        MuroHori: Boolean,
        MuroVert: Boolean,
        
    },
    methods: {
        ClickCelda(){
            this.$emit('CeldaClick', {fila: this.fila, columna: this.columna});
        }, 
    },
};
</script>
<template>
    <div :class="['celda', {'P1': Jugador1, 'P2':Jugador2, 'MuroV': MuroVert, 'MuroH':MuroHori}]" @click="ClickCelda">
        </div>
        <div v-if="MuroHori && MuroVert" class="ContMuro">+</div>
        <div v-else-if="MuroHori" class="ContMuro">|</div>
        <div v-else-if="MuroVert" class="ContMuro">--</div>
        
</template>
<style>
.celda{
    width: 50px;
    height: 50px;
    border: 1px solid black;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;   
   
}
.P1{
    background-color: blue;
    border-radius: 15px;
}

.P2{
    background-color: red;
    border-radius: 15px;
}

.MuroH::before {
  content: '';
  width: 100%;
  height: 8px;
  background-color: black;
  position: absolute;
  bottom: -4px;
  left: 0;
}

.MuroV::after {
    content: '';
    width: 8px;
    height: 100%;
    background-color: black;
    position: absolute;
    right: -5px;
    top: 0;
}

.ContMuro{
    font-size: 0px;
    font-weight: bold;
    color: white;
}


</style>