<script>

    // Firebase imports
    import { FirebaseApp, User, Doc, Collection } from "sveltefire";
    import firebase from "firebase/app";
    import "firebase/firestore";
    import "firebase/auth"; 
    import "firebase/performance";
    import "firebase/analytics";

    // My imports
    import {currentUser} from '/Users/Chris/Projects/Proj10/src/user.js'

    const firebaseConfig = {
      apiKey: "AIzaSyAUb9voHyaLgmuTM-FlUnHKQ28XYnRxiRU",
      authDomain: "proj10-362f0.firebaseapp.com",
      projectId: "proj10-362f0",
      storageBucket: "proj10-362f0.appspot.com",
      messagingSenderId: "1094096035873",
      appId: "1:1094096035873:web:2042e0ea11ae3d6e81c360",
      measurementId: "G-RNFPBWS5SM"
    };

    let email = '';
    let password = '';
    let checkLogin = false;
    
    function signIn() {
        firebase.auth().signInWithEmailAndPassword(email, password).then((user) => {
            // Signed in. Update login flag
        })
        .catch((error) => {
            var errorCode = error.code;
            var errorMessage = error.message;
            alert('Incorrect email and / or password');
        });
    }

    // Init firebase
    firebase.initializeApp(firebaseConfig);

</script>

<main>

  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>

    <h1>Liverpool TAViTRAK</h1>

    <User let:user let:auth> 
        
        <em>Signed in as: {user.email}</em>

        <button on:click={() => auth.signOut()}>Sign Out</button>

        <div slot="signed-out">
          <input type="email" bind:value={email} placeholder="Email">
          <br>
          <input type="password" bind:value={password} placeholder="Password">
          <br>
          <button on:click={signIn}>Login</button>
        </div>
        <hr/>
    </User>
  </FirebaseApp>

</main>


<!-- Styles -->
<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1,
  em {
    color: #ff3e00;
  }

  hr {
    height: 1px;
    border: none;
    background: rgb(195, 195, 195);
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>