<script>
  export default {
    name: 'graphics-card',
    props: {
      daysLeft: {
        default: 0
      },

      spentDaysPercent: {
        default: 0
      },

      previsionMoney: {
        default: 0
      },

      prevision: {
        default: 0
      }
    }
  }
</script>

<template>
  <q-card v-if="data.firstAccess">
    <div style="padding: 20px">
      <h4>R${{spendMoney}}</h4>
      <q-progress
        :percentage="spentDaysPercent"
        :buffer="prevision"
        style="height: 4px"
      />

      Dias restantes {{Math.floor(daysLeft) }}<br>
      Previsão de gastos R${{ decimal(previsionMoney)}}<br>
      {{ prevision }}

      <q-input v-model="data.money" type="number" prefix="R$" stack-label="Dinheiro Atual"/>
      <q-input v-model="data.salary" type="number" prefix="R$" stack-label="Salário"/>

    </div>
  </q-card>
</template>

<style scoped>

</style>
