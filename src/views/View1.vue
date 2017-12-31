<script>
  import
  {
    QCard,
    QCardTitle,
    QCardSeparator,
    QCardMain,
    QCardMedia
  } from 'quasar'

  export default {
    name: 'view-1',
    components: {
      QCard,
      QCardTitle,
      QCardSeparator,
      QCardMain,
      QCardMedia
    },
  }
</script>

<template>
  <div>
    <div class="layout-padding">
      <q-card>
        <q-card-media>
          <img src="~assets/donuts.png">
        </q-card-media>
        <q-card-title>
          Card Title
        </q-card-title>
        <q-card-separator/>
        <q-card-main>
          Card Content
        </q-card-main>
      </q-card>
      <q-card>
        <q-card-media>
          <img src="~assets/donuts.png">
        </q-card-media>
        <q-card-title>
          Card Title
        </q-card-title>
        <q-card-separator/>
        <q-card-main>
          Card Content
        </q-card-main>
      </q-card>
    </div>
  </div>
</template>

<style scoped>

</style>
