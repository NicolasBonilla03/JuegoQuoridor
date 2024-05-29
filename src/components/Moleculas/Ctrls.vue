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
        <button @click="$emit('reiniciar')" class="btn btn-reinicio">Reiniciar</button>
        <button @click="$emit('ActivAccion')" class="btn btn-accion">{{ Accion === 'mover' ? 'Cambiar a Poner  Muro' : 'Cambiar a Moverse'}}</button>
        <div v-if="Accion === 'mover'"></div>
        <div v-if="Accion === 'muro'">
            <p class="texto">Selecciona la orientacion del muro:</p>
            <div class="orien">
            <button @click="$emit('setOrientacionMuro', 'horizontal')" class="btn btn-hor">Horizontal</button>
            <button @click="$emit('setOrientacionMuro', 'vertical')" class="btn btn-ver">Vertical</button>
          </div>
            <p class="texto texto-muro">Muros restantes: {{ Muros }}</p>
        </div>
    </section>
</template>

<style scoped>
.orien{
border-width: 50px;
}

.texto{
  color: white;
  font-size: larger;
}
.texto-muro{
  text-align: center;
}
.Ctrls {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.btn {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #1a1a1a;
  cursor: pointer;
  transition: border-color 0.25s;
}
.btn:hover {
  border-color: #646cff;
}
.btn:focus,
.btn:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}

.btn-reinicio{
  color: red;
}

.btn-accion{
  color: white;
}

.btn-hor{
color: purple;
}

.btn-ver{
    color: green;
    left: 50px;
    position: relative;
}

</style>