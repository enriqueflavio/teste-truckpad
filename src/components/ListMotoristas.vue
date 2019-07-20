<template>
  <div v-if="motoristas" class="row flex q-gutter-md">
    <q-card
      v-for="(motorista, index) in motoristas" :key="index"
      class="my-card bg-secondary text-white"
    >
      <q-card-section>
        <div class="text-h6">{{motorista.name}}</div>
        <div class="text-subtitle2">{{motorista.city}} - {{motorista.state}}</div>
      </q-card-section>

      <q-card-section>
        {{ motorista.addresses.street_name }}
      </q-card-section>

      <q-separator dark />

      <q-card-actions>
        <q-btn flat>Editar</q-btn>
        <q-btn flat>Inativar</q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>
<script>
export default {
  name: 'ListMotoristas',
  props: ['motoristas'],
  data () {
    return {
    }
  }
}
</script>
