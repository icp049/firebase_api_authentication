<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2 v-f = "user">Signed in User: {{user}} </h2>

    <div id="firebaseui-auth-container"></div>
     <div id="loader">Loading...</div>
    
  </div>
</template>

<script>

import { ref } from "vue";
import firebaseConfig from '../firebaseConfig';

import firebase from "firebase/compat/app";

firebase.initializeApp(firebaseConfig);
import * as firebaseui from 'firebaseui'
import 'firebaseui/dist/firebaseui.css'



export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  setup(){
    const user = ref(null);
    const isSignedIn = ref(false);

    const uiConfig = {
     signinFlow: 'popup',
     signinSuccessUrl : 'http://localhost:8080/',
     signInOptions: [
    // List of OAuth providers supported.
    firebase.auth.GoogleAuthProvider.PROVIDER_ID,
    firebase.auth.FacebookAuthProvider.PROVIDER_ID,
    firebase.auth.TwitterAuthProvider.PROVIDER_ID,
    firebase.auth.GithubAuthProvider.PROVIDER_ID
    ],
    callbacks: {
     signInSuccessWithAuthResult: function (authResult)
    {

    user.value = authResult.user.displayName;
    console.log(authResult)
    isSignedIn.value = true;
    console.log('Signed in by user ' + user.value);

    return false;

    }
    }
    }

    var ui = new firebaseui.auth.AuthUI(firebase.auth());

    ui.start('#firebaseui-auth-container' ,uiConfig);

    return{
    user,
    isSignedIn
    }
  
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
