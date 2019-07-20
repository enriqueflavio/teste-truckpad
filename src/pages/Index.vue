<template>
  <q-page class="flex flex-center">
    <list-motoristas :motoristas="listMotoristas" />
  </q-page>
</template>

<style>
</style>

<script>
import json from 'src/json/dados_modelo.json'
import ListMotoristas from 'src/components/ListMotoristas'
export default {
  name: 'PageIndex',
  data () {
    return {
      listMotoristas: json
    }
  },
  components: {
    ListMotoristas
  }
}
</script>
