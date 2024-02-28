<template>
  <div class="padre">
    <div class="header">

      <h1>TALONARIO</h1>
    </div>
    <div class="info">
      <div class="info1">
        <h2>Informacion</h2>
        <div>
          <p>🎁:{{ premio }}</p>
          <p>💰:{{ valor }}</p>
          <p>🏰:{{ loteria }}</p>
          <p>📅:{{ fecha }}</p>
        </div>
      </div>

      <div class="agendar">

        <div class="fila">
          <div class="circulo" v-for="(numero, index) in numerosSeleccionados" :key="index">
            <div class="modal fade" id="exampleModalToggle" aria-hidden="true" aria-labelledby="exampleModalToggleLabel"
              tabindex="-1">
              <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                  <div class="modal-header">
                    
                    <h5 class="modal-title" id="exampleModalToggleLabel">boleta {{ numero}} estado disponible</h5>
                  </div>

                  <div class="modal-footer">
                    <button class="disponible" data-bs-target="#exampleModalToggle2" data-bs-toggle="modal"
                      data-bs-dismiss="modal">adquirir boleta</button>
                  </div>
                </div>
              </div>
            </div>
            <div class="modal fade" id="exampleModalToggle2" aria-hidden="true" aria-labelledby="exampleModalToggleLabel2"
              tabindex="-1">
              <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content" id="comprar">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalToggleLabel2">Modal 2</h5>

                  </div>
                  <div class="modal-body">
                    <input v-model="nombre" type="text" placeholder="nombre de comprador">
                    <input v-model="direccion" type="text" placeholder="Direccion">
                    <input v-model="numeroCel" type="number" placeholder="numero de celular">
                    <select v-model="operaciones">
                      <option value="pagar">pagar</option>
                      <option value="reservar">reservar</option>
                    </select>
                  </div>
                  <div class="modal-footer">
                    <button @click="reservarBoleta()">guardar</button>
                  </div>
                </div>
              </div>
            </div>
            <a class="btn btn-primary" data-bs-toggle="modal" href="#exampleModalToggle" role="button">{{numerosSeleccionados[index]}}</a>
          </div>
        </div>


        <!-- Button trigger modal -->
        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal"
          v-if="mostrarCrearTalonario">
          Crea tu Talonario
        </button>


        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">

          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Crea tu Talonario</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                <input type="text" placeholder="premio" v-model="premio">
                <input type="text" placeholder="valor boleta" v-model="valor">
                <select name="" id="" v-model="loteria">
                  <option value="La culona">La culona</option>
                  <option value="La santander">La santander</option>
                  <option value="La pulga">La pulga</option>
                </select>
                <select name="" id="" v-model="cantidad">
                  <option value="50">50</option>
                  <option value="75">75</option>
                  <option value="100">100</option>
                </select>
                <input type="date" placeholder="loteria" v-model="fecha">
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">cerrar</button>
                <button type="button" class="btn btn-primary" @click="guardar()">guardar</button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="chucho">
        <h2>Acciones</h2>
        <button>Estado</button>
        <!-- Button trigger modal -->
        <!-- Button trigger modal -->
        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#listarBoletasModal">
          📝Listar tus boletas
        </button>

        <!-- Modal para Listar tus boletas -->
        <div class="modal fade" id="listarBoletasModal" tabindex="-1" aria-labelledby="listarBoletasLabel"
          aria-hidden="true">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="otroBoletasLabel">Modal title</h5>

              </div>
              <div class="listar">
                <div class="card" v-for="(item, index) in datos2" :key="index">
                   <p>{{item.numerosreservados  }}</p>
                  <p>{{ item.nombre }}</p>
                  <p>{{ item.direccion }}</p>
                  <p>{{ item.numeroCel }}</p>
                  <p>{{ item.operaciones }}</p>
                  
                </div>
              </div>
              <div class="modal-footer">


              </div>
            </div>
          </div>
        </div>


        <!-- Modal -->
        <div class="modal fade" id="listarBoletasModal" tabindex="-1" aria-labelledby="exampleModalLabel"
          aria-hidden="true">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                <input type="text">
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary">Save changes</button>
              </div>
            </div>
          </div>
        </div>
        <!-- Button trigger modal para Personalizar -->
        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#personalizarModal">
          📑Personalizar
        </button>

        <!-- Modal para Personalizar -->
        <div class="modal fade" id="personalizarModal" tabindex="-1" aria-labelledby="personalizarModalLabel"
          aria-hidden="true">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="personalizarModalLabel">personalizar</h5>

              </div>
              <div class="modal-body" id="personalizar">

                <button @click="cambiaRojo()"> cambiar a rojo</button>
                <button @click="cambiazul()"> cambiar a azul</button>
                <button @click="cambiaNaranja()"> cambiar a naranja</button>
                <button @click="cambiaBlanco()"> cambiar a blanco</button>

              </div>

            </div>
          </div>
        </div>


        <button @click ="descargarPDF()">📩Generar pdf </button>
      </div>

    </div>
    <div class="footer">
      <h3>copyraingt 2023-Todos los derechos reservados</h3>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'
import { jsPDF } from "jspdf";
import autoTable from "jspdf-autotable";

const premio = ref('')
const valor = ref('')
const loteria = ref('')
const cantidad = ref('')
const fecha = ref('')
let mostrarCrearTalonario = ref(true)
const numerosSeleccionados = ref([]);

const nombre = ref('')
const direccion = ref('')
const numeroCel = ref('')
const operaciones = ref('')
const datos2 = ref([]);
const guardar = () => {

  const datos = ref({
    premio: premio.value,
    valor: valor.value,
    loteria: loteria.value,
    cantidad: cantidad.value,
    fecha: fecha.value
  })


  for (let i = 1; i <= cantidad.value; i++) {
    numerosSeleccionados.value.push(i);
  }

  console.log(datos.value)
  console.log(numerosSeleccionados);
  mostrarCrearTalonario = false;


}

jsPDF.autoTable = autoTable;

function descargarPDF() {
  const doc = new jsPDF();
  doc.setFontSize(12);
  doc.text("Resumen de boletas vendias", 10, 10);
  const tableData = datos2.value.map((boleta, index) => [
    boleta.numeroCel,
    boleta.nombre,
    boleta.operaciones,
    boleta.direccion,
  ]);
  doc.autoTable({
    head: [["Boleta", "Nombre", "Teléfono", "Dirección", "Identificación"]],
    body: tableData,
    starty: 20,
  });
  doc.save("vendidas.pdf");
}

// const selecionarboleta=() => {
      
// }
const reservarBoleta = (item,index) => {

const reserva = {
  nombre: nombre.value,
  direccion: direccion.value,
  numeroCel: numeroCel.value,
  operaciones: operaciones.value,


  numerosreservados:parseInt( numerosSeleccionados.value[index])

};
console.log("hola"+reserva.numerosreservados);

// Agregar la reserva al array datos2
datos2.value.push(reserva);

console.log(datos2.value)
// console.log(numerosSeleccionados);
// mostrarCrearTalonario = false;
// console.log('Ocultar:', ocultar.value)

  // const reserva = {
  //   nombre: nombre.value,
  //   direccion: direccion.value,
  //   numeroCel: numeroCel.value,
  //   operaciones: operaciones.value,
    
  // };

  // Agregar la reserva al array datos2
  // datos2.value.push(reserva);



  // console.log(datos2.value)
  // console.log(numerosSeleccionados);
  //   mostrarCrearTalonario = false;
  //  console.log('Ocultar:', ocultar.value)

}

function cambiaRojo() {
  const padre = document.querySelector('.padre')
  padre.style.backgroundColor = 'red'
}
function cambiazul() {
  const padre = document.querySelector('.padre')
  padre.style.backgroundColor = 'blue'
}
function cambiaNaranja() {
  const padre = document.querySelector('.padre')
  padre.style.backgroundColor = '#ffc018'
}
function cambiaBlanco() {
  const padre = document.querySelector('.padre')
  padre.style.backgroundColor = 'white'
}
</script>

<style scoped>
.padre {
  display: grid;
  grid-template-rows: 10vh 80vh 10vh;

}

.header {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ffc018;
  color: white;
}

.footer {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ffc018;
  color: white;
  padding: 10px;
}

.info {
  display: grid;
  grid-template-columns: 20% 60% 20%;
  gap: 10px;
  padding: 30px;

}

.info1 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
  padding: 5px;
  border: 1px solid black;
  width: 80%;
  height: 80%;
  margin-top: 20%;
  border-radius: 10px;
  background-color: #f8f69f;

}

.chucho {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
  padding: 5px;
  border: 2px solid black;
  width: 80%;
  height: 80%;
  margin-top: 20%;
  border-radius: 10px;
  padding: 10px;
  background-color: #f8f69f;
}

.chucho button {
  width: 100%;
  height: 10%;
  color: white;
  background-color: #ff9934;
  border: 2px solid black;
  border-radius: 5px;

}

.agendar {
  display: flex;
  justify-content: center;
  align-items: center;

}

.agendar button {
  width: 20%;

  background-color: #ff9934;
  border-radius: 5px;
  color: black;
}

.modal-body {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 10px;
}

.btn {
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  background-color: #ccc;
  text-align: center;
  line-height: 50px;
  margin: 8px;
  background-color: #ff9934;
  border: 2px solid;

}

.fila {
  display: grid;
  grid-template-columns: repeat(10, 1fr);

}

.modal-header {
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-footer {

  display: flex;
  justify-content: center;
  align-items: center;

}

.modal-footer .disponible {
  width: 100%;
  height: 5vh;
}

.card {
  width: 120px;
  height: 200px;
  display: flex;
  justify-content: center;
  align-items: center;

  background-color: #ccc;
  text-align: center;
  line-height: 50px;
  margin: 8px;
  background-color: #ff9934;
  border: 2px solid;

}

.listar {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  align-items: center;
  height: auto;
  padding: 50px;

}

.modal-content {

  position: relative;
  bottom: 0;
}

#personalizar>* {
  width: 25vh;
  height: 5vh;
}

#personalizar {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

}</style>