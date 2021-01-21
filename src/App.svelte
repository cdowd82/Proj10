<script>

  // Firebase imports
  import { FirebaseApp, User, Doc, Collection } from "sveltefire";
  import firebase from "firebase/app";
  import "firebase/firestore";
  import "firebase/auth"; 
  import "firebase/performance";
  import "firebase/analytics";

  // My imports
  import userBox from '/Users/Chris/Projects/Proj10/src/User.svelte';
  import { onMount } from "svelte";
  import { currentUser } from '/Users/Chris/Projects/Proj10/src/user.js';
  //let logo = '/Users/Chris/Projects/Proj10/src/image.gif';

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
  let showMobileMenu = false; // Show mobile icon and display menu
  
  function signIn() {
      firebase.auth().signInWithEmailAndPassword(email, password).then((user) => {
          // Signed in. Update login flag
          currentUser.set(email);
          console.log($currentUser);
          navItems[4].label = $currentUser;
      })
      .catch((error) => {
          var errorCode = error.code;
          var errorMessage = error.message;
          alert('Incorrect email and / or password');
      });
  }

  // Init firebase
  firebase.initializeApp(firebaseConfig);

   // List of navigation items
  const navItems = [
    { label: "Profile", href: "#" },
    { label: "Admin", href: "#" },
    { label: "Contact", href: "#" },
    { label: "About", href: "#" },
    { label: "User", href: "#" }
  ];
  // Mobile menu click event handler
  const handleMobileIconClick = () => (showMobileMenu = !showMobileMenu);
  // Media match query handler
  const mediaQueryHandler = e => {
    // Reset mobile state
    if (!e.matches) {
      showMobileMenu = false;
    }
  };
  // Attach media query listener on mount hook
  onMount(() => {
    const mediaListener = window.matchMedia("(max-width: 767px)");
    mediaListener.addListener(mediaQueryHandler);
  });

</script>

<main>
  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>
    <User let:user let:auth> 

        <div slot="signed-out">
          <h1>Liverpool TAViTRAK</h1>
          <input type="email" bind:value={email} placeholder="Email">
          <br>
          <input type="password" bind:value={password} placeholder="Password">
          <br>
          <button on:click={signIn}>Login</button>
        </div>

        <nav>
          <div class="inner">
            <div on:click={handleMobileIconClick} class={`mobile-icon${showMobileMenu ? ' active' : ''}`}>
              <div class="middle-line"></div>
            </div>
            <ul class={`navbar-list${showMobileMenu ? ' mobile' : ''}`}>
              {#each navItems as item}
                <li>
                  <a href={item.href}>{item.label}</a>
                </li>
              {/each}
            </ul>
          </div>
        </nav>

        <p>User = {user.email}</p>
        <em>Signed in as: {user.email}</em>
        <br>
        <br>
        <button on:click={() => auth.signOut()}>Sign Out</button>

        <!-- svelte-ignore a11y-missing-attribute -->
        <!--<img src={logo}>-->

        <div class="userBox">
          <p>This is a userBox</p>
        <div>
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

  nav {
    background-color: rgba(0, 0, 0, 0.8);
    font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif;
    height: 45px;
  }

  .inner {
    max-width: 980px;
    padding-left: 20px;
    padding-right: 20px;
    margin: auto;
    box-sizing: border-box;
    display: flex;
    align-items: center;
    height: 100%;
  }

  .mobile-icon {
    width: 25px;
    height: 14px;
    position: relative;
    cursor: pointer;
  }

  .mobile-icon:after,
  .mobile-icon:before,
  .middle-line {
    content: "";
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
    transition: all 0.4s;
    transform-origin: center;
  }

  .mobile-icon:before,
  .middle-line {
    top: 0;
  }

  .mobile-icon:after,
  .middle-line {
    bottom: 0;
  }

  .mobile-icon:before {
    width: 66%;
  }

  .mobile-icon:after {
    width: 33%;
  }

  .middle-line {
    margin: auto;
  }

  .mobile-icon:hover:before,
  .mobile-icon:hover:after,
  .mobile-icon.active:before,
  .mobile-icon.active:after,
  .mobile-icon.active .middle-line {
    width: 100%;
  }

  .mobile-icon.active:before,
  .mobile-icon.active:after {
    top: 50%;
    transform: rotate(-45deg);
  }

  .mobile-icon.active .middle-line {
    transform: rotate(45deg);
  }

  .navbar-list {
    display: none;
    width: 100%;
    justify-content: space-between;
    margin: 0;
    padding: 0 40px;
  }

  .navbar-list.mobile {
    background-color: rgba(0, 0, 0, 0.8);
    position: fixed;
    display: block;
    height: calc(100% - 45px);
    bottom: 0;
    left: 0;
  }

  .navbar-list li {
    list-style-type: none;
    position: relative;
  }

  .navbar-list li:before {
    content: "";
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 1px;
    background-color: #424245;
  }

  .navbar-list a {
    color: #fff;
    text-decoration: none;
    display: flex;
    height: 45px;
    align-items: center;
    padding: 0 10px;
    font-size: 13px;
  }

  @media only screen and (min-width: 767px) {
    .mobile-icon {
      display: none;
    }

    .navbar-list {
      display: flex;
      padding: 0;
    }

    .navbar-list a {
      display: inline-flex;
    }
  }
</style>
