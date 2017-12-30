<script>
  import {
    QLayout,
    QToolbar,
    QToolbarTitle,
    QBtn,
    QIcon,
    QList,
    QListHeader,
    QItem,
    QItemSide,
    QItemMain,
    QTabs,
    QRouteTab,
    QInput,
    QField,
    QDatetime,
    QProgress,
    Dialog,
    QCard,
    QTooltip
  } from 'quasar'

  export default {
    name: 'index',
    components: {
      QLayout,
      QToolbar,
      QToolbarTitle,
      QBtn,
      QIcon,
      QList,
      QListHeader,
      QItem,
      QItemSide,
      QItemMain,
      QTabs,
      QRouteTab,
      QInput,
      QField,
      QDatetime,
      QProgress,
      Dialog,
      QCard,
      QTooltip
    },

    data() {
      return {
        data: {
          firstAccess: false,
          money: '',
          salary: '',
          renovationDay: '',
          startDate: ''
        },
        actualDate: new Date(),
        log: ''
      }
    },

    methods: {
      save() {
        localStorage.setItem('data', JSON.stringify(this.data))
      },

      decimal(value) {
        return parseFloat(Math.round(value * 100) / 100).toFixed(2)
      },

      firstAccessDialog() {
        Dialog.create({
          title: 'Bem-vindo',
          message: 'Por favor, informe sua renda fixa e o dia de renovação',
          noBackdropDismiss: true,
          noEscDismiss: true,
          form: {
            name: {
              type: 'text',
              label: 'Salário (R$)',
              model: this.data.salary
            },

            day: {
              type: 'number',
              label: 'Dia de renovação',
              model: new Date().getUTCDate(),
              min: 1,
              max: 31
            }
          },

          buttons: [
            {
              label: 'Entrar',
              handler: (data) => {
                this.data.salary = data.name
                this.data.renovationDay = data.day
                this.data.startDate = new Date()
                this.data.firstAccess = true
                this.save()
              }
            }
          ]
        })
      }
    },

    computed: {
      daysInMonth() {
        if (this.data.renovationDay < this.actualDate.getDay()) {
          return Math.floor(new Date(this.actualDate.getYear(), this.actualDate.getMonth(), 0).getDate())
        } else {
          return Math.floor(new Date(this.actualDate.getYear(), this.actualDate.getMonth() - 1, 0).getDate())
        }
      },

      daysLeft() {
        return Math.round((this.dateEnd - this.actualDate) / (1000 * 3600 * 24))
      },

      dateEnd() {
        let aux = this.actualDate
        if (this.data.renovationDay !== '') {
          if (this.daysInMonth > 28 || this.data.renovationDay <= 28) {
            let date = (this.actualDate.getMonth() + 1) + '/' + this.data.renovationDay + '/' + (this.actualDate.getYear() + 1900)
            aux = new Date(date)
          }
          else {
            let date = (this.actualDate.getMonth() + 1) + '/' + (this.data.renovationDay - this.daysInMonth) + '/' + (this.actualDate.getYear() + 1900)
            aux = new Date(date)
          }
        }
        aux.setMonth(aux.getMonth() + 1)
        return aux
      },

      spentDaysPercent() {
        return ((this.daysInMonth - this.daysLeft) * 100 / this.daysInMonth)
      }
      ,

      spendMoney() {
        return this.data.salary - this.data.money
      }
      ,

      previsionMoney() {
        return (this.spendMoney / (this.daysInMonth - this.daysLeft)) * this.daysInMonth
      },

      prevision() {
        let result = this.decimal((this.previsionMoney * 100) / this.data.money)
        return result < 100 ? result : 0
      }
    },

    created() {
      if (localStorage.getItem('data')) {
        this.data = JSON.parse(localStorage.getItem('data'))
      }
      else {
        this.firstAccessDialog()
      }

      console.log("register didShow received!");

      var listener = function (e) {
        //log: didShow received! userInfo: {"data":"test"}
        console.log("didShow received! userInfo: " + JSON.stringify(e));
      }

      window.broadcaster.addEventListener("didShow", listener);
    }
  }
</script>

<template>
  <q-layout>
    <q-toolbar>
      <q-toolbar-title>
        Happy Money
      </q-toolbar-title>
    </q-toolbar>

    <q-card v-if="!data.firstAccess">
      <h1>Bem-vindo</h1>
      <q-btn @click="firstAccessDialog()">Configuração Inicial</q-btn>
    </q-card>

    <q-card v-if="data.firstAccess">
      <div style="padding: 20px">
        <h4>R${{spendMoney}}</h4>
        <q-progress
          :percentage="spentDaysPercent"
          :buffer="prevision"
          style="height: 4px"
        />

        Dias restantes: {{Math.floor(daysLeft) }}<br>

        <!--<q-input v-model="data.money" type="number" prefix="R$" stack-label="Dinheiro Atual"/>-->
        <q-input v-model="data.salary" type="number" prefix="R$" stack-label="Salário"/>

      </div>
    </q-card>

    <q-card v-if="data.firstAccess" class="row day-prev" style="padding: 10px">
      <q-field
        icon="wifi"
        label="Wifi network"
        helper="We need your wifi id"
        :error="error2"
        error-label="That's not a valid id number"
      >
        <q-input v-model="text" color="amber" :inverted="error" float-label="Float Label"
                 :after="[{icon: 'arrow_forward', content: true, handler () {}}]"/>
      </q-field>

    </q-card>
    <q-card v-if="data.firstAccess" class="row day-prev" style="padding: 10px">
      <div class="col-4 justify-center">
        <strong>Limite Disponivel</strong>
        R${{ decimal((data.money / daysLeft)) }}
      </div>
      <div class="col-4 justify-center">
        <strong>Limite Restante</strong>
        R${{ decimal((data.money / daysLeft)) }}
      </div>
      <div class="col-4 justify-center">
        <strong>Limite Amanhã</strong>
        R${{ decimal((data.money / (daysLeft - 1))) }}
      </div>
    </q-card>
  </q-layout>
</template>

<style scoped>
  .day-prev {
    text-align: center;
  }
</style>
