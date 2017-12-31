<script>
  import
  {
    QCard,
    QCardActions,
    QIcon,
    QBtn,
    QCardTitle,
    QCardSeparator,
    QCardMain,
    QCardMedia
  } from 'quasar'

  import firebase from 'firebase'

  export default {
    name: 'view-1',
    components: {
      QCard,
      QCardActions,
      QIcon,
      QBtn,
      QCardTitle,
      QCardSeparator,
      QCardMain,
      QCardMedia
    },
    data() {
      return {
        app: null,
        quotes: {}
      }
    },
    created() {
      let config = {
        apiKey: 'AIzaSyCnzSzAxreadbWWmce8rRx0vTIdvDfRz0g',
        authDomain: 'quo-a-quote-manager.firebaseapp.com',
        databaseURL: 'https://quo-a-quote-manager.firebaseio.com',
        projectId: 'quo-a-quote-manager',
        storageBucket: 'quo-a-quote-manager.appspot.com',
        messagingSenderId: '102932102800'
      }
      this.app = firebase.initializeApp(config)

      let db = firebase.database()
      let dbRef = db.ref().child('quotes').child('list')
      dbRef.on('value', snap => {
        this.quotes = snap.val()
      })
      db.getInstance().setPersistence(true)
    },
    beforeDestroy() {
      this.app.delete()
    }
  }
</script>

<template>
  <div>
    <q-card>
      <q-card-actions align="around">
        <q-btn flat round small color="red">
          <q-icon name="edit"/>
        </q-btn>
        <q-btn flat round small color="faded">
          <q-icon name="bookmark"/>
        </q-btn>
        <q-btn flat round small color="faded">
          <q-icon name="share"/>
        </q-btn>
        <q-btn flat round small color="faded">
          <q-icon name="share"/>
        </q-btn>
      </q-card-actions>
    </q-card>
    <q-card v-for="item in quotes">
      <q-card-title>
        {{ item.name }}
      </q-card-title>
      <q-card-separator/>
      <q-card-main>
        Card Content
      </q-card-main>
    </q-card>
  </div>
</template>

<style scoped>

</style>
