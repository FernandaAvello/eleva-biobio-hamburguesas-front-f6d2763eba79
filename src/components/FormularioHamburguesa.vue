<template>

  <!-- Form Creación de Nombre de Hamburguesa -->

  <div class="form-floating mb-3">
    <input
      type="text"
      class="form-control"
      id="floatingInput"
      placeholder="Nombre Hamburguesa"
      v-model="nombre"
    />
    <label for="floatingInput">Nombre Hamburguesa </label>
  </div>

  <!-- Checkbox de Ingredientes -->

  <div class="container">
    <h5 style="text-align: left"><strong>Ingredientes:</strong></h5>
    <br />
    <div class="row">
      <div
        class="col-4 mx-auto form-check form-check-inline"
        v-for="ingrediente in ingredientes"
        :key="ingrediente.nombre"
      >
        <input
          class="form-check-input"
          type="checkbox"
          id="inlineCheckbox1"
          :checked="uncheckIngredientes(ingrediente.nombre)"
          @change="agregarIngrediente(ingrediente)"
        />
        <label class="form-check-label" for="inlineCheckbox1">{{
          ingrediente.nombre
        }}</label>
      </div>
    </div>
    <br />
    <h5 style="text-align: left"><strong>Calorías:</strong> {{ calorias }}</h5>
  </div>
  <div class="modal-footer">
    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
      Cerrar
    </button>
    <button type="button" class="btn btn-primary" @click="guardarHamburguesa" :disabled="deshabilitarBotonCrear">
      Guardar
    </button>
  </div>
</template>

<script>
export default {
  props: ["cerrarModal", "limpiar"],
  data() {
    return {
      nombre: "",
      calorias: 0,
      ingredientesSeleccionados: [],
      ingredientes: [ // Se decidió hacer una lista preestablecida de ingredientes disponibles con su respectiva caloría asociada.
        {
          nombre: "Hamburguesa",
          calorias: 295,
        },
        {
          nombre: "Tomate",
          calorias: 15,
        },
        {
          nombre: "Palta",
          calorias: 160,
        },
        {
          nombre: "Queso",
          calorias: 200,
        },
        {
          nombre: "Cebolla",
          calorias: 40,
        },
        {
          nombre: "Mayonesa",
          calorias: 320,
        },
        {
          nombre: "Pepinillos",
          calorias: 20,
        },
        {
          nombre: "Lechuga",
          calorias: 15,
        },
        {
          nombre: "Huevo frito",
          calorias: 115,
        },
        {
          nombre: "Champiñones",
          calorias: 100,
        },
        {
          nombre: "Choclo",
          calorias: 105,
        },
        {
          nombre: "Tocino",
          calorias: 350,
        },
      ],
    };
  },
  methods: {
    agregarIngrediente(ingrediente) {
      if (
        !this.ingredientesSeleccionados.some( // Se valida que el ingrediente a agregar no esté en la lista de ingredientes seleccionados.
          (ing) => ing === ingrediente.nombre
        )
      ) {
        this.ingredientesSeleccionados.push(ingrediente.nombre);
        this.calorias += ingrediente.calorias;
      } else {
        const index = this.ingredientesSeleccionados.indexOf( // Si no, se busca el índice del ingrediente para removerlo,
          ingrediente.nombre
        );
        this.ingredientesSeleccionados.splice(index, 1); 
        this.calorias -= ingrediente.calorias;
      }
    },
    guardarHamburguesa() {
      const hamburguesa = {
        nombre: this.nombre,
        ingredientes: this.ingredientesSeleccionados,
        calorias: this.calorias,
      };
      this.$http
        .post("https://prueba-hamburguesas.herokuapp.com/burger/", hamburguesa)
        .then(() => {
          this.cerrarModal()
        });
    },

    uncheckIngredientes(nombre) {
      return this.ingredientesSeleccionados.some(
        (ingrediente) => ingrediente === nombre
      );
    },
  },

  computed: {
    deshabilitarBotonCrear() {
      if (
        this.nombre === "" ||
        this.ingredientesSeleccionados.length === 0 ||
        this.calorias === null
      ) {
        return true; // Se deshabilita el Botón al estar las propiedades del objeto vacías.
      }
      return false;
    },
  },
  watch: {
    limpiar(){
      this.nombre= ""
      this.calorias= 0
      this.ingredientesSeleccionados= []
    }
  }
};
</script>

<style>
</style>