<template>
  <section class="src-components-preguntas">
    <div class="jumbotron">
    
      <h2>Preguntas</h2>
      <hr>
      <br>
      <table class="table ">

          <tr>
            <th v-for="(col, index) in getCols" :key="index"> {{ col.toUpperCase() }}</th>
          </tr>

          <tr v-for="(pregunta, index) in preguntas" :key="index" >
            <td v-for="(col, index) in getCols" :key="index"> {{ pregunta[col] }} </td>
          </tr>

        </table>

    </div>
  </section>  
</template>

<script lang="js">
  export default {
    name: 'Preguntas',
    data () {
      return {
        preguntas: [
          { numero: 1, respuestas: ['c'] },
          { numero: 2, respuestas: ['c'] },
          { numero: 3, respuestas: ['b'] }
        ]
      }
    },
    computed: {
      getCols() {
        return Object.keys(this.preguntas[0]);
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="css">
  
  .jumbotron{
    background-color: rgba(168, 27, 27, 0.836);
    color: white;
  }

  hr{
    background-color: #eee;
  }

  .table{
    text-align: center;
    width: 50%;
    color: white;
  }

</style>