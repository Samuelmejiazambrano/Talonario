<template>
  <div class="padre" :style="{ backgroundColor: color }">
    <div class="header" :style="{ backgroundColor: colorHeader }">
      <h1>TALONARIO</h1>
    </div>
    <div class="info">
      <div class="info1" :style="{ backgroundColor: colorcard }">
        <h2>Informacion</h2>
        <div>
          <p>
            <b>🎁:{{ premio }}</b>
          </p>
          <p>
            <b>💰:{{ valor }}</b>
          </p>
          <p>
            <b>🏰:{{ loteria }}</b>
          </p>
          <p>
            <b>📅:{{ fecha }}</b>
          </p>
          <button
            type="button"
            class="editar"
            data-bs-toggle="modal"
            data-bs-target="#exampleModal4"
            v-if="mostrarCrearTalonario2"
            @click="editar()"
          >
            editar
          </button>

          
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
                  <div class="personalizar">
                    <h5 class="modal-title" id="exampleModalToggleLabel">
                      Boleta {{ selectedBoleta }}
                    </h5>
                  </div>
                  <div class="modal-footer">
                    <button
                      class="disponible"
                      data-bs-target="#exampleModalToggle2"
                      data-bs-toggle="modal"
                      data-bs-dismiss="modal"
                      v-if="ocultarbuton"
                    >
                      Adquirir boleta
                    </button>
                    <button
                      class="disponible"
                      data-bs-target="#exampleModalToggle2"
                      data-bs-toggle="modal"
                      data-bs-dismiss="modal"
                      v-else-if="!ocultarbuton"
                    >
                      <b> Observa tu boleta</b>
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
                  <div
                    class="modal-header"
                    :style="{ backgroundColor: 'orange' }"
                  >
                    <h5
                      class="modal-title"
                      id="exampleModalToggleLabel2"
                      v-if="mostrarCrearusuario"
                    >
                      Compra tu Boleta
                    </h5>
                    <h5
                      class="modal-title"
                      id="exampleModalToggleLabel2"
                      v-else-if="mostrarinputs"
                    >
                      <b>Opciones</b>
                    </h5>
                  </div>
                  <div class="modal-body" v-if="mostrarCrearusuario">
                    <form>
                      <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre</label>
                        <input
                          type="text"
                          class="form-control"
                          id="nombre"
                          v-model="nombre"
                        />
                      </div>
                      <div class="mb-3">
                        <label for="direccion" class="form-label"
                          >Dirección</label
                        >
                        <input
                          type="text"
                          class="form-control"
                          id="direccion"
                          v-model="direccion"
                        />
                      </div>
                      <div class="mb-3">
                        <label for="numeroCel" class="form-label"
                          >Número de celular</label
                        >
                        <input
                          type="text"
                          class="form-control"
                          id="numeroCel"
                          v-model="numeroCel"
                        />
                      </div>
                      <div class="mb-3">
                        <label for="operaciones" class="form-label"
                          >Operaciones</label
                        >
                        <select
                          class="form-select"
                          id="operaciones"
                          v-model="operaciones"
                        >
                          <option value="">Seleccione una operación</option>
                          <option value="pagar">pagar</option>
                          <option value="reservar">reservar</option>
                          <option value="disponible">disponible</option>
                        </select>
                      </div>
                    </form>
                  </div>

                  <div class="modal-footer">
                    <button
                      v-if="mostrarCrearusuario"
                      @click="reservarBoleta(index)"
                    >
                      guardar
                    </button>
                    <div v-else-if="mostrarinputs" class="mostrarinputs">
                      <button @click="comprarBoleta()">
                        <b>comprar Boleta</b>
                      </button>

                      <button
                        type="button"
                        class="butoninputs"
                        data-bs-toggle="modal"
                        data-bs-target="#staticBackdrop"
                        @click="
                          mostrarcomprador();
                          mostrarDatosComprador();
                        "
                      >
                        <b>Cliente</b>
                      </button>

                    
                      <button class="butoninputs" @click="liberarBoleta()">
                        <b>liberar</b>
                      </button>
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
              <div class="personalizar" v-if="mostrarCrearusuario">
                <h1 class="modal-title fs-5" id="exampleModalLabel">
                  Crea tu Talonario
                </h1>
              </div>
              <div
                class="personalizar"
                v-if="!mostrarCrearusuario"
                :style="{ backgroundColor: ' LightGreen', borderRadius: '5px' }"
              >
                <h1 class="modal-title fs-5" id="exampleModalLabel">
                  Talonario guardado
                </h1>
              </div>
              <div class="modal-body" v-if="mostrarCrearusuario">
                
                <label >Ingresa el premio:</label>
                <input type="text" placeholder="premio" v-model="premio" />
                   <label >Ingresa el valor de la Boleta:</label>
                <input
                  type="number"
                  placeholder="valor boleta"
                  v-model="valor"
                />
                   <label >Ingresa el nombre de la Loteria:</label>
                <select v-model="loteria">
                  
                  <option value="La culona">La culona</option>
                  <option value="La santander">La santander</option>
                  <option value="La pulga">La pulga</option>
                </select>
                <label >Ingresa la cantidad de Boletas:</label>
                <select name="" id="" v-model="cantidad">
                   
                  <option value="50">50</option>
                  <option value="75">75</option>
                  <option value="100">100</option>
                </select>
                <label >Ingresa la Fecha:</label>
                <input type="date" placeholder="Fecha de sorteo" v-model="fecha" />
              </div>
              <div class="modal-footer" v-if="mostrarCrearusuario">
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
      <div class="chucho" :style="{ backgroundColor: colorcard }">
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
              <div class="personalizar">
                <h5 class="modal-title" id="otroBoletasLabel">
                  Lista de Compradores
                </h5>
              </div>
              <div class="listar">
                <div class="card" v-for="(item, index) in datos2" :key="index">
                  <div class="inputs">
                    <label for="">Boleta:</label>
                    <p>{{ item.selectedBoleta }}</p>
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
              <div class="personalizar">
                <h5 class="modal-title" id="personalizarModalLabel">
                  📑Personalizar
                </h5>
              </div>
              <div class="modal-body" id="personalizar">
                <div>
                  <label>Personaliza el fondo:</label>
                  <input type="color" v-model="color" />
                </div>
                <div>
                  <label>Personaliza Header: </label>
                  <input type="color" v-model="colorHeader" />
                </div>
                <div>
                  <label>Personaliza el footer: </label>
                  <input type="color" v-model="colorfooter" />
                </div>
                <div>
                  <label>Personaliza Cards: </label>
                  <input type="color" v-model="colorcard" />
                </div>
               
              </div>
            </div>
          </div>
        </div>

        <button @click="descargarPDF()">📩Generar pdf</button>
      </div>
    </div>
    <div class="footer" :style="{ backgroundColor: colorfooter }">
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
let formularioCrear = ref(true);
let mostrarCrearTalonario = ref(true);
let mostrarCrearTalonario2 = ref(false);
let mostrarCrearusuario = ref(true);
let ocultarbuton = ref(true);
let mostrarComprador = ref(true);
let mostrarbuton = ref(false);
// let formularioCrearhola = ref(true);
let mostrarinputs = ref(false);
let editar2 = ref(false);
const selectedBoleta = ref({});
let color = ref("white");
let colorHeader = ref("#ff9934");
let colorfooter = ref("#ff9934");
let colorcard = ref("#f8f69f");

const editar = () => {
  formularioCrear.value = true;
  mostrarCrearTalonario2.value = true;
  mostrarCrearusuario.value = true;
};
const compradorSeleccionado = ref(null);

// };
const liberarBoleta = () => {
  const selectedBoletaIndex = datos2.value.findIndex(
    (boleta) => boleta.selectedBoleta === selectedBoleta.value
  );
  if (selectedBoletaIndex >= 0) {
    datos2.value[selectedBoletaIndex].operaciones = "disponible";
    datos2.value.splice(selectedBoletaIndex, 1);
  } else {
    console.error("Boleta no encontrada");
  }
};
const comprarBoleta = (index) => {
  const selectedBoletaIndex = datos2.value.findIndex(
    (boleta) => boleta.selectedBoleta === selectedBoleta.value
  );
  if (selectedBoletaIndex >= 0) {
    datos2.value[selectedBoletaIndex].operaciones = "pagar";
    mostrarCrearusuario.value = false;
  } else {
    console.error("Boleta no encontrada");
  }
};


const mostrarDatosComprador = () => {
  if (!selectedBoleta.value) {
    return;
  }

  const comprador = datos2.value.find(
    (item) => item.selectedBoleta === selectedBoleta.value
  );
  if (comprador) {
    Swal.fire({
      title: "Información del Comprador",
      html: `<p><b>Nombre:</b> ${comprador.nombre}</p>
             <p><b>Dirección:</b> ${comprador.direccion}</p>
             <p><b>Número de teléfono:</b> ${comprador.numeroCel}</p>
             <p><b>Operaciones:</b> ${comprador.operaciones}</p>`,
      icon: "info",
    });
  } else {
    Swal.fire("Error", "No se encontró información para esta boleta.", "error");
  }
};
const mostrarModal = () => {
  mostrarinputs.value = false;
  console.log("Inputs mostrados:", mostrarinputs.value);
};
const mostrarcomprador = () => {
  mostrarComprador.value = true;
  console.log("Inputs mostrados:", mostrarinputs.value);
};
const cerrarcomprador = () => {
  mostrarComprador.value = true;
  console.log("Inputs mostrados:", mostrarinputs.value);
};
// Método para cerrar el modal
const cerrarModal = () => {
  mostrarinputs.value = true;
};
const selectButton = (index) => {
  selectedBoleta.value = numerosSeleccionados.value[index];
  if (
    datos2.value.some(
      (item) =>
        item.selectedBoleta === selectedBoleta.value &&
        (item.operaciones === "pagar" || item.operaciones === "reservar")
    )
  ) {
    mostrarCrearusuario.value = false;
    ocultarbuton.value = false;
  } else {
    mostrarCrearusuario.value = true;
    liberarBoleta(index);
    ocultarbuton.value = true;
  }
};

const getColor = (index) => {
  const boleta = datos2.value.find(
    (item) => item.selectedBoleta === numerosSeleccionados.value[index]
  );
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
const datos = ref([]);

const guardar = () => {
  if (
    !premio.value ||
    !valor.value ||
    !loteria.value ||
    !cantidad.value ||
    !fecha.value
  ) {
    Swal.fire("Por favor completa todos los campos");
  } else if (isNaN(parseInt(cantidad.value)) || parseInt(cantidad.value) <= 0) {
    Swal.fire("La cantidad debe ser un número positivo");
  } else {
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
    mostrarinputs.value = true;
    editar2.value = true;
    mostrarCrearTalonario2.value = true;
    formularioCrear.value = false;
    mostrarCrearusuario.value = false;

    console.log(formularioCrear.value);
  }
};

const totalDineroRecolectado = ref(0);
jsPDF.autoTable = autoTable;

function descargarPDF() {
  let usuariosPagados = datos2.value.filter(
    (usuario) => usuario.operaciones === "pagar"
  );
  const totalDineroRecolectado = usuariosPagados.reduce(
    (total, boleta) => total + boleta.valor2,
    0
  );

  const doc = new jsPDF();
  doc.setFontSize(12);
  doc.text("Resumen de boletas vendidas", 10, 10);

  // Calcula el espacio restante en la página
  let startY = 20; // Inicia en la posición y donde comienza la tabla
  let availableSpace = doc.internal.pageSize.height - startY - 10; // 10 de margen inferior

  const tableData = usuariosPagados.map((item, index) => [
    item.selectedBoleta,
    item.nombre,
    item.numeroCel,
     item.direccion,
    item.operaciones,
   
  ]);

  const totalPages = doc.internal.getNumberOfPages();

  
  const tableHeight = (tableData.length + 1) * 10; 

  if (startY + tableHeight > doc.internal.pageSize.height) {
 
    doc.addPage();
    startY = 20; 
    availableSpace = doc.internal.pageSize.height - startY - 10;
  }

 
  doc.autoTable({
    head: [["Boleta", "Nombre", "Teléfono", "Dirección", "Operacion"]],
    body: tableData,
    startY: startY,
  });

  
  doc.text(`Total dinero recolectado: ${totalDineroRecolectado}`, 10, 80);

  
  doc.save("vendidas.pdf");
}

const reservarBoleta = (index) => {
  if (
    !nombre.value ||
    !direccion.value ||
    !numeroCel.value ||
    !operaciones.value
  ) {
    Swal.fire("Por favor completa todos los campos");
  }

  const reserva = {
    valor2: valor.value,
    nombre: nombre.value,
    direccion: direccion.value,
    numeroCel: numeroCel.value,
    operaciones: operaciones.value,
    selectedBoleta: selectedBoleta.value,
  };

  nombre.value = "";
  direccion.value = "";
  numeroCel.value = "";
  operaciones.value = "";

  console.log("Boleta reservada:", reserva);

  if (
    datos2.value.some(
      (item) =>
        item.selectedBoleta === selectedBoleta.value &&
        (item.operaciones === "pagar" || item.operaciones === "reservar")
    )
  ) {
    alert("Esta boleta ya ha sido reservada o pagada");
  }
  datos2.value.push(reserva);

  console.log("Datos actualizados:", datos2.value);
  selectButton();
  // mostrarCrearusuario.value = false;
  ocultarbuton.value = false;
  mostrarbuton.value = true;
  mostrarCrearusuario.value = false;
};
</script>
<style scoped>
.padre {
  display: grid;
  grid-template-rows: 10vh 100vh 10vh;
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
  /* padding: 5px; */
  position: fixed;
  width: 100%;
  bottom: 0;
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
.modal-body input , select,label{
  border-radius: 10px;
  padding: 5px;
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
  height: 7vh;
}

.card {
  width: auto; 
  height: auto; 
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #ccc; 
  text-align: center;
  line-height: 1.5; 
  margin: 8px;
  background-color: #ff9934; 
  border: 2px solid #000; 
  border-radius: 10px; 
  color: #fff; 
  font-weight: bold; 
  padding: 20px;
}

.card p {
  margin: 8px 0;
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
  width: 35vh;
  height: 7vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ccc;
  border-radius: 10px;
  border: 2px solid;
}

#personalizar {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: Arial, Helvetica, sans-serif;
}
#personalizar input[type="color"] {
  margin-left: 8px;
}
.inputs {
  display: flex;
  justify-content: center;
  align-items: center;
}
.mostrarinputs {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 40px;
}
.mostrarinputs button {
  width: 12vh;
  height: 8vh;
}
.editar {
  width: 100px;
  padding: 2px;
  border-radius: 5px;
}
.personalizar {
  background-color: #ffc018;
  text-align: center;
  padding: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  
}
.personalizar input{
  border-radius: 20px;
}
</style>
