<script setup>
  import {ref, reactive, computed} from "vue"; 

  // computed sirve para coomputar propiedades
  // ref sirve para crear variables o constantes ue sean responsive (que se actualizen en el front-end automaticamente)

  const titulo = "Ejemplos de vue";
  const frutas = [
    {
      nombre:"guayaba",
      precio:300,
      madura:true,
      votos:reactive(0)
    },
    {
      nombre:"banano",
      precio:750,
      madura:true,
      votos:ref(0)
    },
    {
      nombre:"coco",
      precio:500,
      madura:false,
      votos:ref(0)
    },
    {
      nombre:"fresa",
      precio:1000,
      madura:true,
      votos:ref(0)
    }
  ];
  const carrito = ref([]);

  function aumentar(valor){valor.value++;};
  function reducir(valor){valor.value--;};
  function resetear(valor){valor.value = 0;};

  function claseVotos(valor){ // no se recomienda colocar mucha logica en el template por eso la hacemos en el script
    if (valor.value >0 ) return 'positivo'
    else if (valor.value < 0 ) return 'negativo'
    else return 'zero'
  }

  function add(elemento){ // no consigo clonar un objeto :(
    let nombre = elemento.nombre;
    let votos = elemento.votos;
    carrito.value.push(
      {
        nombre:nombre,
        votos:votos
      }
      )
  }

  function blockAdd(frutaDada){
    let consulta = carrito.value.find((fruta) => fruta.nombre == frutaDada.nombre);
    return consulta ? true : false;
  }
</script>

<template>
  <h1>{{ titulo.toUpperCase() }}</h1>
  <h2>Lista de Frutas</h2>
  <ul>
    <template v-for="(fruta, index) in frutas" :key="fruta.nombre">
      <li v-if="fruta.madura">
        <ul >
          {{ index }} -
          <li v-for="(value, prop) in fruta" :key="value">
            <span :class="claseVotos(value)">{{ prop }} : {{ value }}</span>
          </li>
          <button @click="aumentar(fruta.votos)">upVote</button>
          <button v-on:click="reducir(fruta.votos)">downVote</button>
          <button @click="resetear(fruta.votos)">reset</button>
          <button @click="add(fruta)" :disabled="blockAdd(fruta)">Add</button>
        </ul>
      </li>
    </template>
  </ul>
  <h2>Carrito</h2>
  <ul v-for="fruta in carrito">
    <li>{{ fruta.nombre }}s ({{ fruta.votos }})</li>
  </ul>
</template>

<style>
.positivo{
  color: green;
}
.negativo{
  color: red;
}
.zero{
  color: tan;
}
</style>