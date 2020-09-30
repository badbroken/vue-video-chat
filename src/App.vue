<template>
  <Navigation :user="user" @logout="logout" />
  <router-view :user="user" @logout="logout" />
</template>

<script>
import Navigation from "./components/Navigation"
import Firebase from "firebase"
import db from "./db"

export default {
  name: "App",
  // Data is a function which returns some data
  data: function() {
    return {
      user: null
    }
  },
  methods: {
    logout: function() {
      Firebase.auth().signOut().then(() => {
        this.user = null
        this.$router.push("/login")
      })
    }
  },
  // Mounted is a Vue Lifecycle hook to modify the data
  //mounted hook gets our data when we need it
  mounted() {
    Firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.user = user
      }
    })
    db.collection("users") //From our Collection in firestore, we collect the users
      .doc("mx7pPo6psqqCKysZCngD") //The document ID in the firestore
      .get()
      // Anytime we Read a data from firestore, the read data is known as Snapshot
      .then(snapshot => {
        this.user = snapshot.data().name
        // using data() method to get the name from firestore collection
      })
  },
  components: {
    Navigation
  }
}
</script>

<style lang="scss">
$primary: #5f2882;
@import "node_modules/bootstrap/scss/bootstrap";
</style>
