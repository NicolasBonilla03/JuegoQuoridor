<script>
   export default{
    props: {
        msg: String,
    },
   };
</script>
<template>
    <h1 class="titulo">JUEGO QUORIDOR</h1>
    <div class="msj">{{msg}}</div>
</template>
<style>
.msj{
    margin-bottom: 10px;
    font-size: 1.2em;
}

.titulo{
    font-size: 3.2em;
    line-height: 1.1;
}
</style>