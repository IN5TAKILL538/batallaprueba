<script setup>
import axios from "axios";
import { ref } from "vue";

const nombre = ref("");
const image = ref("");
const numpokedex = ref("");
const stats = ref("");

const nombre2 = ref("");
const image2 = ref("");
const numpokedex2 = ref("");
const stats2 = ref("");

const numeros1 = ref([]);
const numeros2 = ref([]);
const numi = ref(0);
const i = ref(0);

const contador1 = ref(0);
const contador2 = ref(0);
const pokeganador = ref("");
const entrenadorganador = ref("");

const statshp = ref("");
const statsatk = ref("");
const statsdf = ref("");
const statsatksp = ref("");
const statsdfsp = ref("");
const statsspeed = ref("");

const statshp2 = ref("");
const statsatk2 = ref("");
const statsdf2 = ref("");
const statsatksp2 = ref("");
const statsdfsp2 = ref("");
const statsspeed2 = ref("");

let imgganador = ref("");

function numerospokedex() {
  numeros1.value = [];
  numeros2.value = [];

  for (let j = 0; j < numi.value; j++) {
    const numeroAleatorio1 = Math.floor(Math.random() * 1000) + 1;
    const numeroAleatorio2 = Math.floor(Math.random() * 1000) + 1;
    numeros1.value.push(numeroAleatorio1);
    numeros2.value.push(numeroAleatorio2);
  }
}

function cambiarpantalla() {
  
  document.body.style.height = "auto";
  document.getElementById("cuerpo").style.display = "flex";
  document.body.style.width = "100%";
  document.body.style.height = "100vh";
  document.getElementById("header").style.display = "none";
  document.getElementById("nextcombat").style.display = "block";
  document.getElementById("resultadolocal").style.display = "block";
  document.getElementById("resultados").style.display = "flex";
  document.getElementById("poke1").style.display = "block";
  document.getElementById("poke2").style.display = "block";
  document.getElementById("nextcombat").textContent = "FIGHT";
  document.getElementById("resetear").style.display = "none";
  document.getElementById("resultadoglobal").style.display = "none";
  document.getElementById("imgpoke1").src = "../src/img/player1gif.gif";
  document.getElementById("imgpoke2").src = "../src/img/player2gif.gif";
  
}
async function listarPokemon() {
  // Mostrar elementos
   document.getElementById("vs").style.display = "block"
  document.getElementById("imgpoke1").style.display = "block"
  document.getElementById("imgpoke2").style.display = "block"
  document.body.style.height = "auto";
  document.getElementById("cuerpo").style.display = "flex";
  document.body.style.width = "100%";
  document.body.style.height = "100vh";
  document.getElementById("header").style.display = "none";
  document.getElementById("nextcombat").style.display = "block";
  document.getElementById("resultadolocal").style.display = "block";
  document.getElementById("resultados").style.display = "flex";
  document.getElementById("poke1").style.display = "flex";
  document.getElementById("poke2").style.display = "flex";
  document.getElementById("nextcombat").textContent = "Siguiente Combate";
  document.getElementById("resetear").style.display = "none";
  document.getElementById("resultadoglobal").style.display = "none";

  numi.value = parseInt(document.getElementById("numpokemon").value);
  if (isNaN(numi.value) || numi.value <= 0) {
    alert("Por favor, selecciona un número válido de Pokémon.");
    return;
  }

  document.getElementById("contador").style.display = "flex";
  document.getElementById("contador2").style.display = "flex";

  numerospokedex();

  if (i.value < numi.value) {
    const url1 = `https://pokeapi.co/api/v2/pokemon/${numeros1.value[i.value]}`;
    const url2 = `https://pokeapi.co/api/v2/pokemon/${numeros2.value[i.value]}`;

    try {
      const { data } = await axios.get(url1);
      nombre.value = data.name;
      numpokedex.value = data.id;
      image.value = data.sprites.front_default;
      statshp.value = data.stats[0].base_stat;
      statsatk.value = data.stats[1].base_stat;
      statsdf.value = data.stats[2].base_stat;
      statsatksp.value = data.stats[3].base_stat;
      statsdfsp.value = data.stats[4].base_stat;
      statsspeed.value = data.stats[5].base_stat;
      stats.value = calculateTotalStats();

      const { data: data2 } = await axios.get(url2);
      nombre2.value = data2.name;
      numpokedex2.value = data2.id;
      image2.value = data2.sprites.front_default;
      statshp2.value = data2.stats[0].base_stat;
      statsatk2.value = data2.stats[1].base_stat;
      statsdf2.value = data2.stats[2].base_stat;
      statsatksp2.value = data2.stats[3].base_stat;
      statsdfsp2.value = data2.stats[4].base_stat;
      statsspeed2.value = data2.stats[5].base_stat;
      stats2.value = calculateTotalStats(true);

      i.value++;
      compararEstadisticas();
    } catch (error) {
      alert("Ocurrió un error al obtener los datos. Por favor, inténtalo de nuevo.");
    }
  } else {
    finalizarCombate();
  }
}

function calculateTotalStats(isSecond = false) {
  const statBase = isSecond ? { atk: statsatk2.value, df: statsdf2.value, spAtk: statsatksp2.value, spDf: statsdfsp2.value, speed: statsspeed2.value } : { atk: statsatk.value, df: statsdf.value, spAtk: statsatksp.value, spDf: statsdfsp.value, speed: statsspeed.value };
  return parseInt(statBase.atk) + parseInt(statBase.df) + parseInt(statBase.spAtk) + parseInt(statBase.spDf) + parseInt(statBase.speed);
}

function compararEstadisticas() {
  const tipoBatalla = document.getElementById("batalla").value;
  let statPokemon1, statPokemon2;

  switch (tipoBatalla) {
    case "total":
      statPokemon1 = parseInt(stats.value);
      statPokemon2 = parseInt(stats2.value);
      break;
    case "hp":
      statPokemon1 = parseInt(statshp.value);
      statPokemon2 = parseInt(statshp2.value);
      break;
    case "attack":
      statPokemon1 = parseInt(statsatk.value);
      statPokemon2 = parseInt(statsatk2.value);
      break;
    case "defense":
      statPokemon1 = parseInt(statsdf.value);
      statPokemon2 = parseInt(statsdf2.value);
      break;
    case "special-attack":
      statPokemon1 = parseInt(statsatksp.value);
      statPokemon2 = parseInt(statsatksp2.value);
      break;
    case "special-defense":
      statPokemon1 = parseInt(statsdfsp.value);
      statPokemon2 = parseInt(statsdfsp2.value);
      break;
    case "speed":
      statPokemon1 = parseInt(statsspeed.value);
      statPokemon2 = parseInt(statsspeed2.value);
      break;
    default:
      return; // Si no hay selección válida, salir
  }

  if (statPokemon1 > statPokemon2) {
    pokeganador.value = `${nombre.value} gana con ${statPokemon1} ${tipoBatalla} sobre ${statPokemon2} de ${nombre2.value}`;
    contador1.value++;
  } else if (statPokemon1 < statPokemon2) {
    pokeganador.value = `${nombre2.value} gana con ${statPokemon2} ${tipoBatalla} contra ${statPokemon1} de ${nombre.value}`;
    contador2.value++;
  } else {
    pokeganador.value = "Es un empate! Tienes otro intento.";
  }
}

function finalizarCombate() {
  entrenadorganador.value = contador1.value > contador2.value ? "El Ganador es el Entrenador #1":
    contador1.value < contador2.value ? "El Ganador es el Entrenador #2" : "EMPATE" ;
    

  document.getElementById("nextcombat").textContent = "Resultado final";
  document.getElementById("nextcombat").style.display = "none";
  document.getElementById("resultadolocal").style.display = "none";
  document.getElementById("resultadoglobal").style.display = "block";
  document.getElementById("resetear").style.display = "block";
}

function reset() {
  // Reiniciar refs y contadores
  i.value = 0;
  contador1.value = 0;
  contador2.value = 0;

  // Reiniciar elementos en la UI
  nombre.value = "";
  image.value = "";
  numpokedex.value = "";
  stats.value = "";

  imgganador.value = "";

  nombre2.value = "";
  image2.value = "";
  numpokedex2.value = "";
  stats2.value = "";
  pokeganador.value = ""
  document.getElementById("vs").style.display = "none"
  document.getElementById("imgpoke1").style.display = "none"
  document.getElementById("imgpoke2").style.display = "none"
  document.getElementById("header").style.display = "flex";
  document.getElementById("iniciar").style.display = "block";
  document.getElementById("cuerpo").style.display = "none";
  document.getElementById("poke1").style.display = "none";
  document.getElementById("poke2").style.display = "none";
  document.getElementById("resultados").style.display = "none";
  document.body.style.width = "100%";
  document.body.style.height = "100vh";
}
</script>

<template>
  <div class="contenedor">


    <div class="header" id="header">
      <img src="./img/remove.png" alt="" id="imgpokebola">
      <select name="numpokemon" id="numpokemon">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
      </select>
      <button id="iniciar" @click="cambiarpantalla()">INICIAR</button>

    </div>

    <div class="cuerpo" id="cuerpo">

      <div class="pokemon" id="poke1">
        <div class="contador" id="contador">
          <h1 class="txtcontador">{{ contador1 }}</h1>
        </div>
        <div class="contenedorname">
          <div id="name1">{{ nombre }}</div>
        </div>
        <img :src="image" alt="" class="imgpoke" id="imgpoke1">
      </div>
      <div class="pokemon">
        <img src="https://i.pinimg.com/originals/38/e6/8c/38e68c4baccbd17cc50aa752810a1301.gif" alt="" id="vs">
      </div>
      <div class="pokemon" id="poke2">
        <div class="contador" id="contador2">
          <h1 class="txtcontador">{{ contador2 }}</h1>
        </div>
        <div class="contenedorname">
          <div id="name2">{{ nombre2 }}</div>
        </div>
        <img :src="image2" alt="" class="imgpoke" id="imgpoke2">
      </div>
    </div>
    <div class="resultados" id="resultados">
      <div class="selecion">
        <select name="tipobatalla" id="batalla">
          <option value="total">TOTAL</option>
          <option value="hp">HP</option>
          <option value="attack">ATAQUE</option>
          <option value="defense">DEFENSA</option>
          <option value="special-attack">ATAQUE ESPECIAL</option>
          <option value="special-defense">DEFENSA ESPECIAL</option>
          <option value="speed">VELOCIDAD</option>
        </select>
      </div>
      <div class="btnnext">

        <button id="nextcombat" @click="listarPokemon()">Combate</button>
      </div>
      <button id="resultadolocal">{{ pokeganador }}</button> <br>
      <button id="resultadoglobal">{{ entrenadorganador }}</button>

      <button id="resetear" @click="reset()">RESET</button>
      
    </div>


  </div>
</template>

<style>
#app {
  width: 100%;
  height: 100%;
}

body {
  height: 100vh;
  width: 100%;
  background-image: url(./img/estadio.jpeg);
  background-position: center;
  background-size: cover;
}

#name1 {
  color: white;
}
#imgganador{
  height: 200px;
  width: 200px;
}

#name2 {
  color: white;
}

.contador {
  display: none;
  align-items: center;
  justify-content: center;
  font-family: cursive;
}
#contador{
  background-image: url(./img/player1.png);
  background-position: center;
  background-size: cover;
  width: 120px;
  height: 80px;
}
#contador2{
  background-image: url(./img/player2.png);
  background-position: center;
  background-size: cover;
  width: 120px;
  height: 80px;
}

.txtcontador {
  font-size: 70px;
  font-family: cursive;
  text-align:end;
  color: rgb(4, 245, 36);
  
}

.contenedorname {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.pokemon {
  align-items: center;
  justify-content: center;
  text-align: center;
}

.contenedor {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: 100%;
  width: 100%;
  padding: 0%;
  margin: 0%;
}

.selecion {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
}

#name1,
#name2 {
  font-family: cursive;
  background-color: black;
  border-radius: 50%;
  opacity: 0.7;
  width: 120px;
}

#vs {
  
  height: 60px;
  width: 60px;
  opacity: 0.6;
}

#poke1 {
  
  display: flex;
  flex-direction: column;
}

#poke2 {
  display: flex;
  flex-direction: column;
}

.cuerpo {
  display: none;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}

.resultados {
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}

.header {
  display: flex;
  background-image: url(https://i.pinimg.com/originals/9a/d8/e5/9ad8e5a396290ad68e26523220048512.gif);
  background-position: center;
  background-size: cover;
  width: 400px;
  height: 200px;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

#imgpokebola {
  width: 150px;
  height: 150px;
}

.imgpoke {
 
  width: 260px;
  height: 260px;
}

#numpokemon {
  width: 50px;
  height: 20px;
}

#batalla {
  width: 100px;
  height: 20px;
}

#nextcombat {
  display: none;
  font-family: cursive;
}

#resetear:hover {
  background: #318aac;
  color: #7a0505 !important;
  background: linear-gradient(to top, white, black);
  transition: background 0.3s;

}

.btnnext {
  display: flex;
  justify-content: center;
  align-items: center;
}

@media (max-width: 660px) {
  .imgpoke {
    width: 120px;
    height: 120px;
  }
}

#resultadolocal,
#resultadoglobal {
  display: none;
  font-family: cursive;
  background-color: black;
  color: white;
}

#iniciar:hover {
  background: #318aac;
  color: #fbfafd !important;
  background: linear-gradient(to right, rgba(246, 80, 52, 255), rgba(13, 17, 49, 255));
  transition: background 0.3s;


}
</style>