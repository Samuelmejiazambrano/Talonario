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
          <div
            class="circulo"
            v-for="(numero, index) in numerosSeleccionados"
            :key="index"
           >
            <div
              class="modal fade"
              id="exampleModalToggle"
              aria-hidden="true"
              aria-labelledby="exampleModalToggleLabel"
              tabindex="-1"
             >
              <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalToggleLabel">
                      Boleta {{ selectedBoleta }} estado disponible
                    </h5>
                  </div>
                  <div class="modal-footer">
                    <button
                      class="disponible"
                      data-bs-target="#exampleModalToggle2"
                      data-bs-toggle="modal"
                      data-bs-dismiss="modal"
                     
                    >
                      Adquirir boleta
                    </button>
                  </div>
                </div>
              </div>
            </div>

            <div
              class="modal fade"
              id="exampleModalToggle2"
              aria-hidden="true"
              aria-labelledby="exampleModalToggleLabel2"
              tabindex="-1"
            >
              <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content" id="comprar">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalToggleLabel2">
                      Compra tu Boleta
                    </h5>
                  </div>
                 <div class="modal-body">
  <form v-if="mostrarCrearusuario">
    <div class="mb-3">
      <label for="nombre" class="form-label">Nombre</label>
      <input type="text" class="form-control" id="nombre" v-model="nombre">
    </div>
    <div class="mb-3">
      <label for="direccion" class="form-label">Dirección</label>
      <input type="text" class="form-control" id="direccion" v-model="direccion">
    </div>
    <div class="mb-3">
      <label for="numeroCel" class="form-label">Número de celular</label>
      <input type="text" class="form-control" id="numeroCel" v-model="numeroCel">
    </div>
    <div class="mb-3">
      <label for="operaciones" class="form-label">Operaciones</label>
      <select class="form-select" id="operaciones" v-model="operaciones">
        <option value="">Seleccione una operación</option>
        <option value="pagar">pagar</option>
        <option value="reservar">reservar</option>
        <option value="disponible">disponible</option>
      </select>
    </div>
  </form>
</div>

                  <div class="modal-footer">
                   <button v-if="mostrarCrearusuario"  @click="reservarBoleta(index)">guardar</button>
                   <div  v-else-if=" mostrarinputs" class="mostrarinputs">
                    
<button @click="comprarBoleta(index)">comprar</button>
<!-- Button trigger modal -->
<button type="button" class="butoninputs" data-bs-toggle="modal" data-bs-target="#staticBackdrop" @click="mostrarDatosComprador(index)">
  comprador
</button>

<!-- Modal -->
<div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="staticBackdropLabel">Información del Comprador</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" @click="cerrarModal()"></button>
        </div>
     <div class="modal-body" v-if="compradorSeleccionado">
          <p>Nombre: {{ compradorSeleccionado.nombre }}</p>
          <p>Dirección: {{ compradorSeleccionado.direccion }}</p>
          <p>Número de Celular: {{ compradorSeleccionado.numeroCel }}</p>
          <p>Operación: {{ compradorSeleccionado.operaciones }}</p>
        </div>

        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="cerrarModal()">Cerrar</button>
        </div>
      </div>
    </div>
  </div>
  
                     <button class="butoninputs" @click="liberarBoleta(index)">liberar</button>

                   </div>
                  
                 

                  </div>
                </div>
              </div>
            </div>
            <div>
              <a
                class="btn btn-primary"
                data-bs-toggle="modal"
                href="#exampleModalToggle"
                role="button"
                :style="{
                  backgroundColor: getColor(index),
                }"
                @click="selectButton(index)"
                >{{ numerosSeleccionados[index] }}</a
              >
            </div>
          </div>
        </div>

        <!-- Button trigger modal -->
        <button
          type="button"
          class="btn btn-primary"
          data-bs-toggle="modal"
          data-bs-target="#exampleModal4"
          v-if="mostrarCrearTalonario"
        >
          Crea tu Talonario
        </button>

        <!-- Modal -->
        <div
          class="modal fade"
          id="exampleModal4"
          tabindex="-1"
          aria-labelledby="exampleModalLabel4"
          aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">
                  Crea tu Talonario
                </h1>
                <button
                  type="button"
                  class="btn-close"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                ></button>
              </div>
              <div class="modal-body">
                <input type="text" placeholder="premio" v-model="premio" />
                <input type="number" placeholder="valor boleta" v-model="valor" />
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
                <input type="date" placeholder="loteria" v-model="fecha" />
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  class="btn btn-secondary"
                  data-bs-dismiss="modal"
                >
                  cerrar
                </button>
                <button
                  type="button"
                  class="btn btn-primary"
                  @click="guardar()"
                  
                >
                  guardar
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="chucho">
        <h2>Acciones</h2>
        <button>Estado</button>

        <button
          type="button"
          class="btn btn-primary"
          data-bs-toggle="modal"
          data-bs-target="#listarBoletasModal"
        >
          📝Listar tus boletas
        </button>

        <!-- Modal para Listar tus boletas -->
        <div
          class="modal fade"
          id="listarBoletasModal"
          tabindex="-1"
          aria-labelledby="listarBoletasLabel"
          aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="otroBoletasLabel">Lista de Compradores</h5>
              </div>
              <div class="listar" >
                <div class="card" v-for="(item, index) in datos2" :key="index">
                  <div class="inputs">
                  <label for="">Boleta:</label>
                  <p>{{ item.selectedBoleta}}</p>
                  </div>
                   <div class="inputs">
                  <label for="">Nombre:</label>
                  <p>{{ item.nombre }}</p>
                  </div>
                   <div class="inputs">
                  <label for="">direccion:</label>
                  <p>{{ item.direccion }}</p>
                  </div>
                    <div class="inputs">
                  <label for="">direccion:</label>
                  <p>{{ item.direccion }}</p>
                  </div>
                      <div class="inputs">
                  <label for="">telefono:</label>
                 <p>{{ item.numeroCel }}</p>
                  </div>
                      <div class="inputs">
                  <label for="">Estado:</label>
                   <p>{{ item.operaciones }}</p>
                  </div>
                
                </div>
              </div>
              <div class="modal-footer"></div>
            </div>
          </div>
        </div>

        <!-- Modal -->
        <div
          class="modal fade"
          id="listarBoletasModal2"
          tabindex="-1"
          aria-labelledby="exampleModalLabel"
          aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                <button
                  type="button"
                  class="btn-close"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                ></button>
              </div>
              <div class="modal-body">
                <input type="text" />
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  class="btn btn-secondary"
                  data-bs-dismiss="modal"
                >
                  Close
                </button>
                <button type="button" class="btn btn-primary">
                  Save changes
                </button>
              </div>
            </div>
          </div>
        </div>
        <!-- Button trigger modal para Personalizar -->
        <button
          type="button"
          class="btn btn-primary"
          data-bs-toggle="modal"
          data-bs-target="#personalizarModal"
        >
          📑Personalizar
        </button>

        <!-- Modal para Personalizar -->
        <div
          class="modal fade"
          id="personalizarModal"
          tabindex="-1"
          aria-labelledby="personalizarModalLabel"
          aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="personalizarModalLabel">
                  personalizar
                </h5>
              </div>
              <div class="modal-body" id="personalizar">
                <button @click="cambiaRojo()">cambiar a rojo</button>
                <button @click="cambiazul()">cambiar a azul</button>
                <button @click="cambiaNaranja()">cambiar a naranja</button>
                <button @click="cambiaBlanco()">cambiar a blanco</button>
              </div>
            </div>
          </div>
        </div>

        <button @click="descargarPDF()">📩Generar pdf</button>
      </div>
    </div>
    <div class="footer">
      <h3>copyraingt 2023-Todos los derechos reservados</h3>
    </div>
    
  </div>
</template>

<script setup>
import { ref } from "vue";
import { jsPDF } from "jspdf";
import autoTable from "jspdf-autotable";

const premio = ref("");
const valor = ref("");
const loteria = ref("");
const cantidad = ref("");
const fecha = ref("");
const formularioDisponible = ref(true);
let mostrarCrearTalonario = ref(true);
let mostrarCrearusuario = ref(true)
let mostrarinputs = ref(false);
const selectedBoleta = ref({});



const compradorSeleccionado = ref(null);
// const liberarBoleta = () => {
//    mostrarCrearusuario.value = true;
//   operaciones.value = "";
// };
const liberarBoleta = (index) => {
  if (index >= 0 && index < datos2.value.length) {
    const boleta = datos2.value[index]; // Accede al objeto de datos en la posición del índice
    if (boleta && boleta.operaciones) {
      boleta.operaciones = "disponible"; // Actualiza la operación en el objeto de datos
    }
  } else {
    console.error('Índice de boleta inválido');
  }
};

const comprarBoleta = (index) => {
  if (index >= 0 && index < datos2.value.length) {
    const boleta = datos2.value[index]; // Accede al objeto de datos en la posición del índice
    if (boleta && boleta.operaciones) {
      boleta.operaciones = "pagar"; // Actualiza la operación en el objeto de datos
    }
  } else {
    console.error('Índice de boleta inválido');
  }
};





const mostrarDatosComprador = (index) => {
  if (index >= 0 && index < datos2.value.length) {
    const comprador = datos2.value[index];
    
    compradorSeleccionado.value = comprador;
    console.log('Comprador seleccionado:', compradorSeleccionado.value[index]); // Verificar el comprador seleccionado
    mostrarModal(); 
  } else {
    console.error('Índice de comprador inválido');
  }
};



// Método para mostrar el modal
const mostrarModal = () => {
  mostrarinputs.value = false;
};

// Método para cerrar el modal
const cerrarModal = () => {
  mostrarinputs.value = true;
};
const selectButton = (index) => {
  selectedBoleta.value = numerosSeleccionados.value[index];
  if (datos2.value.some(item => item.selectedBoleta === selectedBoleta.value && (item.operaciones === "pagar" || item.operaciones === "reservar"))) {
    mostrarCrearusuario.value = false;
       
    
  } else {
    mostrarCrearusuario.value = true;
  }
  
};

const getColor = (index) => {
  const boleta = datos2.value.find(item => item.selectedBoleta === numerosSeleccionados.value[index]);
  if (boleta && boleta.operaciones) {
    return boleta.operaciones === "pagar"
      ? "blue"
      : boleta.operaciones === "reservar"
      ? "red"
      : "#ff9934";
  } else {
    return "#ff9934"; // Color por defecto
  }
};



const numerosSeleccionados = ref([]);

const nombre = ref("");
const direccion = ref("");
const numeroCel = ref("");
const operaciones = ref("");
const datos2 = ref([]);
const datos=ref([])

const guardar = () => {
 
  if (!premio.value || !valor.value || !loteria.value || !cantidad.value || !fecha.value) {
   Swal.fire("Por favor completa todos los campos"); 
   
  }else

if (isNaN(parseInt(cantidad.value)) || parseInt(cantidad.value) <= 0) {
      Swal.fire("La cantidad debe ser un número positivo");  
  
  }

  const dato = {
    premio: premio.value,
    valor: valor.value,
    loteria: loteria.value,
    cantidad: cantidad.value,
    fecha: fecha.value,
  };
 datos.value.push(dato);
  numerosSeleccionados.value = [];

  for (let i = 1; i <= cantidad.value; i++) {
    numerosSeleccionados.value.push(i);
  }

  console.log(datos);
  console.log(numerosSeleccionados);
  mostrarCrearTalonario.value = false;
  mostrarinputs.value=true
};

const totalDineroRecolectado = ref(0); 
jsPDF.autoTable = autoTable;

function descargarPDF() {
    const totalDineroRecolectado = datos2.value.reduce((total, boleta) => total + boleta.valor2, 0);
  const doc = new jsPDF();
  doc.setFontSize(12);
  doc.text("Resumen de boletas vendidas", 10, 10);
  
  // Calcula el espacio restante en la página
  let startY = 20; // Inicia en la posición y donde comienza la tabla
  let availableSpace = doc.internal.pageSize.height - startY - 10; // 10 de margen inferior
  
  const tableData = datos2.value.map((item, index) => [
    item.selectedBoleta,
    item.numeroCel,
    item.nombre,
    item.operaciones,
    item.direccion,
    totalDineroRecolectado
  ]);
  
  const totalPages = doc.internal.getNumberOfPages();
  
  // Calcula la altura total de la tabla
  const tableHeight = (tableData.length + 1) * 10; // Número de filas * altura de fila (10)
  
  if (startY + tableHeight > doc.internal.pageSize.height) {
    // Si la tabla excede el espacio disponible, añade una nueva página
    doc.addPage();
    startY = 20; // Restablece la posición inicial en la nueva página
    availableSpace = doc.internal.pageSize.height - startY - 10; // Recalcula el espacio disponible
  }
  
  // Genera la tabla
  doc.autoTable({
    head: [["Boleta", "Nombre", "Teléfono", "Dirección", "identiicacion","total a pagar"]],
    body: tableData,
    startY: startY,
  });
  
  // Calcula y muestra el total

  const lastPageHeight = doc.internal.pageSize.height - (totalPages > 1 ? 10 : 0);
  doc.text(`Total dinero recolectado: ${totalDineroRecolectado}`, 10, lastPageHeight - 10);
  
  // Guarda el PDF
  doc.save("vendidas.pdf");
}


const reservarBoleta = (index) => {
  if (!nombre.value || !direccion.value || !numeroCel.value || !operaciones.value) {
   Swal.fire("Por favor completa todos los campos"); 
  }

  const reserva = {
    valor2:valor.value,
    nombre: nombre.value,
    direccion: direccion.value,
    numeroCel: numeroCel.value,
    operaciones: operaciones.value,
    selectedBoleta: selectedBoleta.value,
  };

  console.log("Boleta reservada:", reserva);

  if (datos2.value.some(item => item.selectedBoleta === selectedBoleta.value && (item.operaciones === "pagar" || item.operaciones === "reservar"))) {
    alert('Esta boleta ya ha sido reservada o pagada');
  }
  datos2.value.push(reserva);

  console.log("Datos actualizados:", datos2.value);
  selectButton();
};


function cambiaRojo() {
  const padre = document.querySelector(".padre");
  padre.style.backgroundColor = "red";
}
function cambiazul() {
  const padre = document.querySelector(".padre");
  padre.style.backgroundColor = "blue";
}
function cambiaNaranja() {
  const padre = document.querySelector(".padre");
  padre.style.backgroundColor = "#ffc018";
}
function cambiaBlanco() {
  const padre = document.querySelector(".padre");
  padre.style.backgroundColor = "white";
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
  grid-template-columns: repeat(11, 1fr);
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
  width: auto; /* Ancho de la tarjeta */
  height: auto; /* Altura de la tarjeta */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #ccc; /* Color de fondo */
  text-align: center;
  line-height: 1.5; /* Altura de línea */
  margin: 8px;
  background-color: #ff9934; /* Color de fondo personalizado */
  border: 2px solid #000; /* Borde de la tarjeta */
  border-radius: 10px; /* Radio de borde */
  color: #fff; /* Color del texto */
  font-weight: bold; /* Grosor de la fuente */
  padding:20px ;
}

.card p {
  margin: 8px 0; /* Margen interno de los párrafos dentro de la tarjeta */
}

.listar {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  align-items: center;
  height: auto;
  padding: 50px;
}

.modal-content {
  position: relative;
  bottom: 0;
}

#personalizar > * {
  width: 25vh;
  height: 5vh;
}

#personalizar {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.inputs{
  display: flex;
  justify-content: center;
  align-items: center;
}
.mostrarinputs {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
.mostrarinputs button{
  width: 120%;
}

</style>
