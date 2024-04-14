<script setup>
 import { ref, reactive, onMounted } from 'vue'
 const monedas = ref([
      { codigo: 'USD', texto: 'Dolar de Estados Unidos'},
      { codigo: 'MXN', texto: 'Peso Mexicano'},
      { codigo: 'EUR', texto: 'Euro'},
      { codigo: 'GBP', texto: 'Libra Esterlina'},
  ])
  const criptomonedas = ref([])
  const cotizar = reactive({
    moneda:'',
    criptomoneda:''
  })

  onMounted(()=>{
    const url = 'https://min-api.cryptocompare.com/data/top/mktcapfull?limit=3&tsym=USD';
    fetch(url)
    .then(resppuesta=> resppuesta.json())
    .then(({Data})=>{
      criptomonedas.value = Data
    })
  })
</script>

<template>
   <div class="contenedor">
     <h1 class="titulo">Cotizador de <span>Criptomonedas</span></h1>
     <div class="contenido">
      <form class="formulario">
        <div class="campo">
            <label for="moneda">Moneda:</label>
            <select 
            name="moneda" 
            id="moneda"
            v-model="cotizar.moneda"
            >
                <option value="">-- Selecciona --</option>
                <option 
                  v-for="moneda in monedas" 
                  :value="moneda.codigo"
                  >{{ moneda.texto }}</option>
            </select>
        </div>

        <div class="campo">
            <label for="cripto">Criptomoneda:</label>
            <select 
            name="cripto" 
            id="cripto"
            v-model="cotizar.criptomoneda"
            >
                <option value="">-- Selecciona --</option>
                <option 
                  v-for="criptomoneda in criptomonedas" 
                  :value="criptomoneda.CoinInfo.Name"
                  >{{ criptomoneda.CoinInfo.FullName }}</option>
            </select>
        </div>
        <input type="submit" value="Cotizar" />

      </form>
     </div>
   </div>
</template>