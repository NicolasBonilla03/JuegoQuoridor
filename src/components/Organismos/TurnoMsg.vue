<script>
   export default{
    props: {
        msg: String,
    },
   };
</script>
<template>
    <h1>JUEGO QUORIDOR</h1>
    <div class="msj">{{msg}}</div>
</template>
<style>
.msj{
    margin-bottom: 10px;
    font-size: 1.2em;
}
</style>