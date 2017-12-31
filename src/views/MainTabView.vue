<script>
  import {
    QLayout,
    QToolbar,
    QToolbarTitle,
    QTabs,
    QRouteTab,
    QBtn,
    QIcon,
    QItemSide,
    QItemMain,
    QListHeader
  } from 'quasar'

  export default {
    name: 'index',
    components: {
      QLayout,
      QToolbar,
      QToolbarTitle,
      QTabs,
      QRouteTab,
      QBtn,
      QIcon,
      QItemSide,
      QItemMain,
      QListHeader
    }
  }
</script>

<template>
  <!--Ajuste de layout fixado-->
  <!--Hh-->
  <!--Ff-->
  <q-layout
    view='lHr lpr lFr'>
    <q-toolbar slot="header">
      <q-toolbar-title>
        Happy Money
      </q-toolbar-title>
    </q-toolbar>

    <router-view />

    <q-tabs slot="footer">
      <q-route-tab slot="title" icon="play_circle_outline" to="/" replace label="Play with Layout" />
      <q-route-tab slot="title" icon="view_array" to="/view2" replace label="Drawer Panels" />
    </q-tabs>
  </q-layout>
</template>

<style scoped>
</style>
