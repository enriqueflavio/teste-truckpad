<template>
  <div v-if="motoristas" class="row column q-gutter-md">
    <q-btn class="float-buttom" color="primary" fab icon="fas fa-plus" @click="abrirCadastro" />
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
            <div class="col text-h6 ellipsis">{{motorista.id ? 'Atualizar':'Cadastrar'}} Motorista</div>
            <div class="col-auto text-grey q-pt-md">
              <q-btn round color="negative" class="absolute-top-right" icon="fas fa-times" v-close-popup />
            </div>
          </div>
        </q-card-section>

        <q-card-section class="row flex flex-center">
          <div class="col-9">
            <q-input class="q-mt-md" outlined v-model="motorista.name" label="Nome" />
            <q-input mask="##/##/####" class="q-mt-md" outlined v-model="motorista.birth_date" label="Data Nascimento" />
            <q-input class="q-mt-md" outlined v-model="motorista.city" label="Cidade" />
            <q-input class="q-mt-md" outlined v-model="motorista.state" label="Estado" />
            <q-input
              class="q-mt-md"
              mask="##########"
              outlined
              v-model="motorista.documents.cnh.number"
              label="CNH"
            />
            <q-input
              class="q-mt-md"
              mask="###.###.###-##"
              outlined
              v-model="motorista.documents.cpf.number"
              label="CPF"
            />
            <q-btn
              v-if="!atualizandoMotorista"
              @click="cadastrar"
              color="primary"
              class="q-my-lg full-width"
              label="Cadastrar"
            />
            <q-btn
              v-else
              @click="atualizar(motorista.id)"
              color="primary"
              class="q-my-lg full-width"
              label="Atualizar"
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
        name: '',
        birth_date: '',
        city: '',
        state: '',
        documents: {
          cnh: {
            number: ''
          },
          cpf: {
            number: ''
          }
        }
      },
      atualizandoMotorista: false,
      modal: {
        novoMotorista: false
      }
    }
  },
  methods: {
    abrirCadastro () {
      this.motorista = {}
      this.motorista.documents = {}
      this.motorista.documents.cnh = {}
      this.motorista.documents.cpf = {}
      this.modal.novoMotorista = true
    },
    cadastrar () {
      const novoMotorista = {
        'id': this.motoristas.length + 1,
        'isAtivo': true,
        'name': this.motorista.name,
        'birth_date': this.motorista.birth_date,
        'state': this.motorista.state,
        'city': this.motorista.city,
        'addresses': {
          'name': '',
          'state': this.motorista.state,
          'country': 'BR',
          'neighborhood': 'CENTRO',
          'city': this.motorista.city,
          'complement': '',
          'postal_code': '',
          'street_name': ''
        },
        'documents': {
          'cnh': {
            'expires_at': '',
            'country': 'BR',
            'number': this.motorista.documents.cnh.number,
            'doc_type': 'CNH',
            'category': ''
          },
          'cpf': {
            'country': 'BR',
            'number': this.motorista.documents.cpf.number,
            'doc_type': 'CPF'
          }
        }
      }
      this.motoristas.push(novoMotorista)
      this.modal.novoMotorista = false
    },
    editar (motorista) {
      if (motorista) {
        this.motorista = { ...motorista }
        if (!motorista.documents.cnh) {
          this.motorista.documents.cnh = {}
          this.motorista.documents.cnh.number = ''
        }
        if (!motorista.documents.cpf) {
          this.motorista.documents.cpf = {}
          this.motorista.documents.cpf.number = ''
        }
        this.modal.novoMotorista = true
        this.atualizandoMotorista = true
      }
    },
    atualizar (id) {
      const editMotorista = {
        'name': this.motorista.name,
        'birth_date': this.motorista.birth_date,
        'state': this.motorista.state,
        'city': this.motorista.city,
        'addresses': {
          'name': '',
          'state': this.motorista.state,
          'country': 'BR',
          'neighborhood': 'CENTRO',
          'city': this.motorista.city,
          'complement': '',
          'postal_code': '',
          'street_name': ''
        },
        'documents': {
          'cnh': {
            'expires_at': '',
            'country': 'BR',
            'number': this.motorista.documents.cnh.number,
            'doc_type': 'CNH',
            'category': ''
          },
          'cpf': {
            'country': 'BR',
            'number': this.motorista.documents.cpf.number,
            'doc_type': 'CPF'
          }
        }
      }
      this.motoristas.map(motorista => {
        if (motorista.id === id) {
          motorista = { ...editMotorista }
        }
      })
      this.modal.novoMotorista = false
      this.atualizandoMotorista = false
    },
    ativarinativar (id) {
      this.motoristas.map(motorista => {
        if (motorista.id === id) {
          motorista.isAtivo = !motorista.isAtivo
        }
      })
    }
  }
}
</script>
<style lang="stylus" scoped>
>>>.q-card
  max-width 172px
  min-width 172px
  min-height 200px
.absolute-top-right
  top 8px
  right 5px
  transform scale(.7)
.float-buttom
  position fixed
  bottom 20px
  right 35px
</style>
