<template>
  <div class="principal">
    <div class="cuerpo" v-show="mostrarCuerpo">
      <div class="cabeza">
        <h2 class="h2">Configura tu talonario</h2>
      </div>
      <input
        class="Caja"
        type="number"
        placeholder="Premio del talonario"
        v-model="premio"
      />
      <input
        class="Caja"
        type="number"
        placeholder="precio de 1 Boleta"
        v-model="valorboleta"
      />
      <div class="lotes">
        <label for="options">Seleccione una Loteria:</label>
        <select class="Caja" v-model="loteria">
          <option value="Loteria Santander">Loteria Santander</option>
          <option value="Loteria de Bogota">Lotería de Bogotá</option>
          <option value="Loteria de la Cruz Roja">Lotería de la Cruz Roja</option>
          <option value="Loteria del Tolima">Lotería del Tolima</option>
          <option value="Lotería de Medellín ">Lotería de Medellín</option>
        </select>
      </div>
      <div class="lotes">
        <label for="options">Seleccione la cantidad de boletas:</label>
        <select class="Caja" v-model="selectedOption" @change="selectOption">
          <option value="2">0-99</option>
          <option value="3">0-999</option>
          <option value="4">0-9999</option>
        </select>
      </div>
      <div class="cuerpo_fecha">
        <label for="fecha"> Fecha: </label>
        <input type="date" id="fecha" class="Caja" v-model="fecha" />
      </div>
      <input type="button" class="Boton" value="guardar" @click="guardar_rifa" />
      <h3 :style="alerta === 'Formulario enviado correctamente'">
        {{ alerta }}
      </h3>
    </div>
    <div class="division" v-show="mostrarDemas">
      <div class="datos">
        <h1>Información</h1>
        <div class="datosbody">
          <p class="pdatos">🏆 {{ premio }}</p>
          <p class="pdatos">💵 {{ valorboleta }}</p>
          <p class="pdatos">🏛️ {{ loteria }}</p>
          <p class="pdatos">📅 {{ fecha }}</p>
          <input
            type="button"
            class="Botonrojo"
            value="editar✏️"
            @click="ocultarDemasDivs()"
          />
        </div>
      </div>

      <div class="talonario">
        <div v-if="balls.length" class="balls-container">
          <template v-for="ball in balls" :key="ball.number">
            <div
              @click="showForm(ball)"
              class="ball"
              :style="{
                backgroundColor:
                  ball.state_ball === 'comprada'
                    ? 'blue'
                    : ball.state_ball === 'mora'
                    ? 'red'
                    : 'darkgreen',
              }"
              data-number="{{ ball.number }}"
            >
              {{ ball.number }}
              <span class="ball-state">{{ ball.state }}</span>
            </div>
          </template>
        </div>
      <div v-if="selectedBall" class="form-container">
          <h2>Balota {{ selectedBall.number }}</h2>
            <input class="Caja" type="text" id="owner" v-model="selectedBall.owner" placeholder="Nombre">
            <input class="Caja" type="text" id="phone" v-model="selectedBall.phone" placeholder="Telefono">
            <input class="Caja" type="text" id="address" v-model="selectedBall.address" placeholder="Direccion">
            <select class="Caja" id="state_ball" v-model="selectedBall.state_ball">
              <option value="libre" selected>Libre</option>
              <option value="comprada">Comprada</option>
              <option value="mora">Mora</option>
            </select>
            <button @click="buyBall()" class="Caja">Guardar</button>
            
        </div>
        
      </div>

      <div class="extras">
        <div class="datos">
          <h1>Acciones</h1>
          <div class="datosbody">
            <input type="button" class="Boton3" value="Estado" />
            <input type="button" class="Boton3" value="Listar Tus boletas" />
            <input type="button" class="Boton3" value="Personaliza tu talonario" />
            <input
              type="button"
              class="Boton3"
              @click="generatePDF()"
              value="Generar PDF"
            />
          </div>
        </div>
      </div>
    </div>

    <div class="pdf-content" v-show="show_PDF">
      <h2>Total</h2>
      <table class="tabla-total">
        <tr>
          <th>Valor boleta</th>
          <th>Cantidad comprada</th>
          <th>Total</th>
        </tr>
        <tr>
          <td>{{ valorboleta }}</td>
          <td>{{ balls.filter((ball) => ball.state_ball === "comprada").length }}</td>
          <td>
            {{
              valorboleta * balls.filter((ball) => ball.state_ball === "comprada").length
            }}
          </td>
        </tr>
      </table>
      <p>Fecha del sorteo: {{ fecha }}</p>
      <p>Nombre de la lotería: {{ loteria }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import jsPDF from "jspdf";
import html2canvas from "html2canvas";

let mostrarCuerpo = ref(true);
let mostrarDemas = ref(false);
let selectedOption = ref(null);
const balls = ref([]);
const selectedBall = ref(null);
const state_ball = ref(null);
let show_PDF = ref(false);
let alerta = ref("");
const datos = ref([]);
let premio = ref("");
let valorboleta = ref("");
let loteria = ref("");
let fecha = ref("");


function ocultarDemasDivs() {
  mostrarCuerpo.value = true;
  mostrarDemas.value = false;
}
//GUARDAR DATOS DEL TALONARIO
function ocultarAlerta() {
  setTimeout(() => {
    alerta.value = "";
  }, 3500);
}

function guardar_rifa() {
  if (premio.value === "") {
    alerta.value = "El campo del premio no puede estar vacio";
    ocultarAlerta();
  } else if (premio.value < 50) {
    alerta.value = "El premio es muy bajo para una rifa";
    ocultarAlerta();
  } else if (valorboleta.value == "") {
    alerta.value = "El campo del valor de la boleta no puede estar vacio";
    ocultarAlerta();
  } else if (valorboleta.value < 50) {
    alerta.value = "El valor del boleta es muy bajo para la rifa";
    ocultarAlerta();
  } else if (loteria.value == "") {
    alerta.value = "Seleccione una loteria";
    ocultarAlerta();
  } else if (fecha.value === "") {
    alerta.value = "Seleccione una fecha";
    ocultarAlerta();
  } else {
    datos.value.push({
      premio: premio.value,
      valorboleta: valorboleta.value,
      loteria: loteria.value,
      fecha: fecha.value,
    });
    console.log(datos);
    mostrarDemasDivs();
  }
}

function mostrarDemasDivs() {
  mostrarCuerpo.value = false;
  mostrarDemas.value = true;
}

// Vizcayaaa
const selectOption = () => {
  selectedOption.value = parseInt(selectedOption.value);
  generateBalls();
};

const generateBalls = () => {
  balls.value = Array.from(
    { length: Math.pow(10, selectedOption.value) },
    (_, index) => ({
      number: index,
      owner: "",
      state: "", // Añade un estado para cada balota
    })
  );
};

const showForm = (ball) => {
  selectedBall.value = ball;
};

const buyBall = () => {
  selectedBall.value = null;
  console.log("entrando al boton");
};

const generatePDF = () => {
  const element = document.querySelector(".pdf-content");

  html2canvas(element).then((canvas) => {
    const imgData = canvas.toDataURL("image/png");
    const doc = new jsPDF();
    doc.addImage(imgData, "JPEG", 10, 10);
    doc.save("balotas.pdf");
  });
};
</script>

<style scoped>
body,h1,h2,h3,input,select {
  margin: 0;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}



.pdf-content{
  display: grid;
  position: absolute;
}
.principal {
  background-color: antiquewhite;
  height: 100vh;
  display: grid;
}

.cuerpo {
  background-color: white;
  height: auto;
  width: 350px;
  border: 4px solid black;
  border-radius: 10px;
  position: absolute;
  place-self: center;
  top: 50px;
  display: grid;
  justify-content: center;
}

.cabeza {
  height: 50px;
  width: 350px;
  background: greenyellow;
  text-align: center;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  display: grid;
  align-items: center;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.datos {
  justify-self: center;
  background-color: #86b539c1;
  height: 400px;
  width: 300px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
}
.datosbody {
  background-color: white;
  width: 90%;
  height: auto;
  margin-left: 5%;
  margin-bottom: 5%;
  text-align: left;
  display: grid;
  align-items: center;
  border-radius: 10px;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
}
.Boton3 {
  width: 80%;
  background: #9998d7;
  color: white;
  padding: 8px;
  border-radius: 4px;
  margin-bottom: 10px;
  border: 1px solid #747474;
  font-family: "calibri";
  font-size: 18px;
  margin-top: 3%;
  color: rgb(7, 7, 7);
  justify-self: center;
  text-align: center;
  cursor: pointer;
}

.Caja {
  width: 80%;
  background: #ffffff;
  padding: 8px;
  border-radius: 4px;
  margin-bottom: 16px;
  border: 1px solid #747474;
  font-family: "calibri";
  font-size: 18px;
  margin-top: 3%;
  color: rgb(7, 7, 7);
  justify-self: center;
  text-align: center;
}

.lotes {
  display: grid;
  margin: 0;
  align-items: center;
  place-self: center;
  width: 100%;
}

.cuerpo_fecha {
  display: grid;
}

label {
  display: flex;
  justify-content: center;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}

.Boton {
  background-color: rgb(187, 187, 187);
  width: 100px;
  justify-self: center;
  height: 30px;
  margin-bottom: 2%;
  cursor: pointer;
  border-radius: 10px;
}

.Botonrojo {
  background-color: rgb(221, 8, 8);
  width: auto;
  justify-self: center;
  height: 30px;
  margin-bottom: 2%;
  cursor: pointer;
  border-radius: 10px;
}

.division {
  display: grid;
  grid-template-columns: 25% auto 25%;
  justify-content: center;
  align-content: center;
}

.datos {
  justify-self: center;
  background-color: #d0cfcf;
  height: 400px;
  width: 300px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
}

.datosbody {
  background-color: white;
  width: 90%;
  height: auto;
  margin-left: 5%;
  margin-bottom: 5%;
  text-align: left;
  display: grid;
  align-items: center;
  border-radius: 10px;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
}

.pdatos {
  margin-left: 5%;
  font-size: 25px;
  padding: 10px;
  margin: 0;
}

.ball {
  width: 50px;
  height: 50px;
  background-color: #143076;
  color: white;
  border-radius: 50%;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
  cursor: pointer;
  margin-left: 2%;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
}

.balls-container {
  display: flex;
  flex-wrap: wrap;
}

.form-container {
  display: grid;
  position: absolute;
  left: 500px;
  background-color: white;
  height: 350px;
  width: 400px;
  padding: 5px;
  border-radius: 15px;
  text-align: center;
  bottom: 10px;
  border: solid 4px black;
  
  
}

.ball-form {
  display: flex;
  flex-direction: row;
  max-width: 300px;
  margin: auto;
}

label {
  margin-bottom: 5px;
}

input,
button {
  margin-bottom: 10px;
  padding: 5px;
  border-radius: 5px;
}



button {
  background-color: lightgreen;
  color: black;
  border: 2px solid darkgreen;
  cursor: pointer;
}

.columnas {
  display: flex;
  flex-direction: column;
}

.columna-izquierda,
.columna-derecha {
  width: 30%;
}

.tabla-total {
  width: 50%;
  border-collapse: collapse;
}

.tabla-total th,
.tabla-total td {
  border: 1px solid black;
  padding: 5px;
}
</style>

