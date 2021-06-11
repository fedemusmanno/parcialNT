<template>
  <section class="src-components-form">
    <div class="jumbotron">
    
      <h2>Notas</h2>
      <hr>
      <br>
    
      <vue-form :state="formState" @submit.prevent="enviar()">

        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            name="nombre"
            class="form-control"
            autocomplete="off"
            v-model.trim="formData.nombre"
            required
            :minlength="nomApLengthMin"
            :maxlength="nomApLengthMax"
            sin-espacios-medios
          >
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="sin-espacios-medios" class="alert alert-warning mt-1">
              No se permiten espacios intermedios
            </div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere como mínimo {{ nomApLengthMin }} caracteres
            </div>
            <div v-if="formData.nombre.length == nomApLengthMax" class="alert alert-warning mt-1">
              Este campo permite como máximo {{ nomApLengthMax }} caracteres
            </div>
          </field-messages>  
        </validate>
        <br>

        <validate tag="div">
          <label for="apellido">Apellido</label>
          <input
            type="text"
            id="apellido"
            name="apellido"
            class="form-control"
            autocomplete="off"
            v-model.trim="formData.apellido"
            required
            :minlength="nomApLengthMin"
            :maxlength="nomApLengthMax"
            sin-espacios-medios
          >
          <field-messages name="apellido" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="sin-espacios-medios" class="alert alert-warning mt-1">
              No se permiten espacios intermedios
            </div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere como mínimo {{ nomApLengthMin }} caracteres
            </div>
            <div v-if="formData.apellido.length == nomApLengthMax" class="alert alert-warning mt-1">
              Este campo permite como máximo {{ nomApLengthMax }} caracteres
            </div>
          </field-messages>  
        </validate>
        <br>

        <validate tag="div">
          <label for="nota">Nota</label>
          <input
            type="number"
            id="nota"
            name="nota"
            class="form-control"
            autocomplete="off"
            v-model.number="formData.nota"
            required
            :min="notaMin"
            :max="notaMax"
          >
          <field-messages name="nota" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>            
            <div slot="min" class="alert alert-danger mt-1">La nota mínima es de {{notaMin}}</div>            
            <div slot="max" class="alert alert-danger mt-1">La nota máxima permitida es de {{notaMax}}</div>  
          </field-messages>  
        </validate>
        <br>

        <button class="btn btn-info btn-block my-3" :disabled="formState.$invalid" type="submit">Enviar</button>

      </vue-form>

      <div v-if="registros.length">
        <table class="table table-dark">

          <tr>
            <th v-for="(col, index) in getCols" :key="index"> {{ col.toUpperCase() }}</th>
          </tr>

          <tr v-for="(registro, index) in registros" :key="index" >
            <td v-for="(col, index) in getCols" :key="index" :style="{ color: getColor(registro[col]) }"> {{ registro[col] }} </td>
          </tr>

          <tr class="prom">
            <th>Promedio</th>
            <th :style="{ color: getColor(promedio) }">{{promedio}}</th>
          </tr>
        </table>
      </div >
      
    </div>
  </section>  
</template>

<script lang="js">
  export default {
    name: 'Form',
    props: [],
    data() {
      return{
        formData : this.getInicialData(),
        formState : {},
        nomApLengthMin : 3,
        nomApLengthMax : 15,
        notaMin : 0,
        notaMax : 10,
        registros : [],
        notas : [],
      }
    },
    methods:{
      getInicialData(){
        return{
          nombre: '',
          apellido: '',
          nota: '',
        }
      },
      calcularPromedio(){
        let totalAcum = 0
        this.notas.forEach (function(nota){
        totalAcum += nota;
         });
        this.promedio = totalAcum / this.registros.length
      },
      getColor(nota){
        let color = ''
        if(nota <= 10) color = 'green'
        if(nota <= 6) color = 'yellow'
        if(nota <= 3) color = 'red'
        return color
      },
      enviar(){
        const cargaNota = {
          'nombre y apellido': this.formData.nombre +' ' + this.formData.apellido,
          nota: this.formData.nota,
        }
        this.registros.push(cargaNota)
        this.notas.push(this.formData.nota)
        this.calcularPromedio()
        this.formData = this.getInicialData()
        this.formState._reset()
      },
    },
    computed: {
      getCols() {
        return Object.keys(this.registros[0])
      },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="css">
  
  .jumbotron{
    background-color: rgb(78, 86, 138);
    color: white;
  }

  hr{
    background-color: #eee;
  }

  .prom{
    background-color: rgb(15, 15, 15);
    color: white;
  }

</style>