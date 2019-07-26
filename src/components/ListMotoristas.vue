<template>
  <div v-if="motoristas" class="row column q-gutter-md">
    <q-btn color="primary" label="Cadastrar Motorista" @click="modal.novoMotorista = true" />
    <div class="row flex q-gutter-md">
      <q-card
        v-for="(motorista, index) in motoristas" :key="index"
        :class="`${motorista.isAtivo ? 'bg-secondary':'bg-accent'} my-card text-white`"
      >
        <q-card-section>
          <div class="text-h6">{{motorista.name}}</div>
          <div class="text-subtitle2">{{motorista.city}} - {{motorista.state}}</div>
        </q-card-section>

        <q-card-section>
          {{ motorista.addresses.street_name }}
        </q-card-section>

        <q-separator dark />

        <q-card-actions class="absolute-bottom">
          <q-btn @click="editar(motorista)" :disable="!motorista.isAtivo" flat>Editar</q-btn>
          <q-btn @click="ativarinativar(motorista.id)" flat>
            {{motorista.isAtivo ? 'Inativar': 'Ativar'}}
          </q-btn>
        </q-card-actions>
      </q-card>
    </div>
    <q-dialog v-model="modal.novoMotorista">
      <q-card style="width: 450px; max-width: 80vw;">
        <q-card-section>
          <div class="row no-wrap items-center">
            <div class="col text-h6 ellipsis">Cadastro Motorista</div>
            <div class="col-auto text-grey q-pt-md">
              <q-btn round color="negative" class="absolute-top-right" icon="fas fa-times" v-close-popup />
            </div>
          </div>
        </q-card-section>

        <q-card-section class="row flex flex-center">
          <div class="col-9">
            <q-input class="q-mt-md" outlined v-model="motorista.name" label="Nome" />
            <q-input class="q-mt-md" outlined v-model="motorista.birth_date" label="Data Nascimento" />
            <q-input class="q-mt-md" outlined v-model="motorista.city" label="Cidade" />
            <q-input class="q-mt-md" outlined v-model="motorista.state" label="Estado" />
            <q-input
              class="q-mt-md"
              outlined
              v-model="motorista.documents[0].number"
              label="CNH"
            />
            <q-input
              class="q-mt-md"
              outlined
              v-model="motorista.documents[1].number"
              label="CPF"
            />
            <q-btn
              @click="cadastrar"
              color="primary"
              class="q-my-lg full-width"
              label="Cadastrar"
            />
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>
<script>
export default {
  name: 'ListMotoristas',
  props: ['motoristas'],
  data () {
    return {
      motorista: {
        nome: '',
        dataNascimento: '',
        cidade: '',
        estado: '',
        documents: [
          {
            number: ''
          },
          {
            number: ''
          }
        ]
      },
      modal: {
        novoMotorista: false
      }
    }
  },
  methods: {
    cadastrar () {
      const novoMotorista = {
        'id': this.motoristas.length + 1,
        'isAtivo': true,
        'name': this.motorista.nome,
        'birth_date': this.motorista.dataNascimento,
        'state': this.motorista.estado,
        'city': this.motorista.cidade,
        'addresses': {
          'name': '',
          'state': this.motorista.estado,
          'country': 'BR',
          'neighborhood': 'CENTRO',
          'city': this.motorista.cidade,
          'complement': '',
          'postal_code': '',
          'street_name': ''
        },
        'documents': [
          {
            'expires_at': '',
            'country': 'BR',
            'number': this.motorista.documents[0].number,
            'doc_type': 'CNH',
            'category': ''
          },
          {
            'country': 'BR',
            'number': this.motorista.documents[1].number,
            'doc_type': 'CPF'
          }
        ]
      }
      this.motoristas.push(novoMotorista)
      this.modal.novoMotorista = false
    },
    ativarinativar (id) {
      this.motoristas.map(motorista => {
        if (motorista.id === id) {
          motorista.isAtivo = !motorista.isAtivo
        }
      })
    },
    editar (motorista) {
      if (motorista) {
        this.motorista = { ...motorista }
        console.log(this.motorista)
        // if (motorista.documents[0].doc_type === 'CNH') {
        //   this.motorista.documents[0].number = ''
        // }
        // if (!motorista.documents[1].number) {
        //   this.motorista.documents[1].number = ''
        // }
        this.modal.novoMotorista = true
      }
    }
  }
}
</script>
<style lang="stylus" scoped>
>>>.q-card
  min-height 200px
.absolute-top-right
  top 8px
  right 5px
  transform scale(.7)
</style>
