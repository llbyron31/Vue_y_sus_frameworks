<template>
<div>
    <v-layout :wrap="true">
    <v-flex xs12>
      <v-card>
        <v-date-picker 
          v-model="fecha"
          full-width
          locale="es-cl"
          :min="minimo"
          :max="maximo"
          @change="getDolar(fecha)"
        >

        </v-date-picker>
      </v-card>
      <v-card color="error" dark>
        <v-card-text class="display-1 text-center">
          {{valor}}
        </v-card-text>
      </v-card>
    </v-flex>

  </v-layout>

</div>
</template>

<script>
  import axios from "axios";
  export default {
    data() {
      return {
        fecha: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
        //Configuro la fecha mínima
        minimo: '1984',
        //Configuro la fecha máxima
        maximo: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
        valor: null
      }
    },
    methods:{
      async getDolar(dia){
        let arrayfecha = dia.split(['-'])
        let ddmmyy = arrayfecha[2]+'-'+arrayfecha[1]+'-'+arrayfecha[0];
        
        try {

          let datos = await axios.get(`https://mindicador.cl/api/dolar/${ddmmyy}`)
          // si el dia a consultar es sábado o domingo, no arroja error en la consola
          if(datos.data.serie.length > 0){
            this.valor = await datos.data.serie[0].valor
          }else{
            this.valor = 'Sin resultados'
          }
        } catch (error) {
          // console.log(error)
        }
        finally{
          
        }

      }
    },
    created(){
      this.getDolar(this.fecha)
    }
  }
</script>
