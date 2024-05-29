<script>
   export default{
    props: {
        msg: String,
    },
   };
</script>
<template>
    <h1 class="titulo">JUEGO QUORIDOR</h1>
    <div class="titulo titulo-msg">{{msg}}</div>
</template>
<style>
.titulo-msg{
    margin-bottom: 10px;
    font-size: 1.2em;
}

.titulo{
    color: white;
    font-size: 3.2em;
    line-height: 1.1;
}
</style>