<script>

  // Firebase imports
  import { FirebaseApp, User, Doc, Collection } from "sveltefire";
  import firebase from "firebase/app";
  import "firebase/firestore";
  import "firebase/auth"; 
  import "firebase/performance";
  import "firebase/analytics";

  // Svelte imports
  import Workup from '/Users/Chris/Projects/Proj10/src/Workup.svelte';
  import Evaluation from '/Users/Chris/Projects/Proj10/src/Evaluation.svelte';
  import Procedure from '/Users/Chris/Projects/Proj10/src/Procedure.svelte';
  import Complete from '/Users/Chris/Projects/Proj10/src/Complete.svelte';
  import Search from '/Users/Chris/Projects/Proj10/src/Search.svelte';
  import { fade, fly } from 'svelte/transition'
  import { onMount } from "svelte";
  
  // My imports
  import { currentUser } from '/Users/Chris/Projects/Proj10/src/user.js';
  const logo = "Images/lphArms.png";

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

  // Sign in auth
  function signIn() {
      firebase.auth().signInWithEmailAndPassword(email, password).then((user) => {
          // Signed in. Update login flag
          currentUser.set(email);
          navItems[4].label = $currentUser;
      })
      .catch((error) => {
          var errorCode = error.code;
          var errorMessage = error.message;
          alert('Incorrect email and / or password');
      });
  }

  /*   alerts for button testing   */
  function enterWorkup() {
    alert("Workup entered");
  }

  function enterEval() {
    alert("Eval entered");
  }

  function enterProcedure() {
    alert("Procedure entered");
  }

  function enterComp() {
    alert("Comp entered");
  }

  // Init firebase
  firebase.initializeApp(firebaseConfig);

   // List of navigation items
  const navItems = [
    { label: "Profile", href: "#"},
    { label: "Admin", href: "#" },
    { label: "Contact", href: "#" },
    { label: "About", href: "#" },
    { label: currentUser, href: "#"}
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

<main transition:fly="{{y:200, duration: 1000}}">
  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>
    <User let:user let:auth> 

        <div in:fade="{{y:1000, duration: 2000}}" slot="signed-out">
          <div><img bottom-margin="0" src={logo} alt="LPH Logo" width="108" height="138"></div>
          <h1 style="strong">TAVR<sup>2</sup></h1>
          <br>
          <input top-margin="0" type="email" bind:value={email} placeholder="Email">
          <br>
          <input type="password" bind:value={password} placeholder="Password">
          <br>
          <button on:click={signIn}>Login</button>
        </div>

        <div transition:fly="{{y:1000, duration: 1200}}">
          <nav>
            <div class="inner">
              <div on:click={handleMobileIconClick} class={`mobile-icon${showMobileMenu ? ' active' : ''}`}>
                <div class="middle-line"></div>
              </div>
              <ul class={`navbar-list${showMobileMenu ? ' mobile' : ''}`}>
                <li><a href="#"> <div class="nav-tavr">Liverpool-TAVR<sup>2</sup></div></a></li>
                <li><a href={navItems[0].href}>{navItems[0].label}</a></li>
                <li><a href={navItems[1].href}>{navItems[1].label}</a></li>
                <li><a href={navItems[2].href}>{navItems[2].label}</a></li>
                <li><a href={navItems[3].href}>{navItems[3].label}</a></li>
                <li><a href={navItems[4].href}>
                  <button class="logout-Btn" on:click={() => auth.signOut()}>
                    Logout: {navItems[4].label}</button></a></li>
              </ul>
            </div>
          </nav>
        </div>
     
        <div class="container-1">
          <div class="box-1" transition:fade="{{duration: 2800}}">
            <button class="enter-workup-btn" on:click={enterWorkup}><Workup/>
          </div>
          <div class="box-2" transition:fade="{{duration: 2400}}">
            <button class="enter-eval-btn" on:click={enterEval}><Evaluation/>
          </div>
        </div>

        <div class="container-1">
          <div class="box-3" transition:fade="{{duration: 2000}}">
            <button class="enter-comp-btn" on:click={enterComp}><Complete/>
          </div>
          <div class="box-4" transition:fade="{{duration: 1800}}">
            <button class="enter-procedure-btn" on:click={enterProcedure}><Procedure/>
          </div>
        </div>

        <div class="container-2">
          <div class="box-5" transition:fly="{{y: -1000, duration: 1200}}">
            <Search/>
          </div>
        </div>

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
    height: 100%;
  }

  h1,
  em {
    color: hsla(0, 100%, 35%, 0.89);
  }

  hr {
    height: 1px;
    border: none;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

  nav {
    background-color: rgba(0, 0, 0, 0.8);
    font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif;
    height: 50px;
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

    .navbar-list a:hover {
      background-color: rgba(63, 62, 62, 0.986);
    }

  }

  .logout-Btn {
    background-color: #343436;
    font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif;
    border: none;
    color: white;
    padding-top: 1em
  }

  h1 {
    font-size: 75px;
    font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif;
    color: hsla(0, 100%, 35%, 0.89);
    margin-top: 0;
  }

   /* only apply purgecss on utilities, per Tailwind docs */
  /* purgecss start ignore */
  @tailwind base;
  @tailwind components;
  /* purgecss end ignore */

  @tailwind utilities;

  @media(min-width:468px){
    .container-1 {
      display:flex;
      height: 42%;
      text-align: center;
    }
    
    .container-2 {
      display:flex;  
      text-align: center;
      justify-content:space-between;
    }
  }
  
  .container-3 {
    display:flex;  
    flex-wrap:wrap;
    justify-content:space-between;
  }
  
  .container-1 div, .container-2 div, .container-3 div{
    border: 2px rgb(53, 51, 51) solid;
    align-content: center;
  }
  
  .box-1{
    flex:1;
    order:1;
    background-color: rgb(253, 86, 86);
    align-content: center;
    
  }
  
  .box-2{
    flex:1;
    order:2;
    background-color: rgb(255, 197, 72);
    align-content: center;
  }
  
  .box-3{
    flex:1;
    order: 1;
    background-color: rgb(59, 76, 235);
    align-content: center;
  }

  .box-4{
    flex:1;
    order: 2;
    background-color: rgb(60, 196, 89);
    align-content: center;
  }

  .box-5{
    flex:1;
    order: 1;
    background-color: rgba(26, 26, 26, 0.986);
    align-content: center;
  }
  
  .container-2-box{
    flex-basis: 27%;  
  }
  
  .container-3-box{
    flex-basis:12%;
  }

  .nav-tavr {
      color:hsla(0, 97%, 77%, 0.89);
      font-size: 14px;
      font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif;
  }

  .container-1 .box-1:hover {
    background-color: rgb(253, 128, 128);
  }

  .container-1 .box-2:hover {
    background-color: rgb(255, 215, 129);
  }

  .container-1 .box-3:hover {
    background-color: rgb(101, 114, 238);
  }

  .container-1 .box-4:hover {
    background-color: rgb(118, 202, 137);
  }

  .container-2 .box-5:hover {
    background-color: rgba(63, 62, 62, 0.986);
  }

  .enter-workup-btn {
    width: 100%;
    height: 100%;
    background-color: rgb(253, 86, 86);
    align-content: center;
    border: 0px;
  }

  .enter-eval-btn {
    width: 100%;
    height: 100%;
    background-color: rgb(255, 197, 72);
    align-content: center;
    border: 0px;
  }

  .enter-procedure-btn {
    width: 100%;
    height: 100%;
    background-color: rgb(60, 196, 89);
    align-content: center;
    border: 0px;
  }

  .enter-comp-btn {
    width: 100%;
    height: 100%;
    background-color: rgb(59, 76, 235);
    align-content: center;
    border: 0px;
  }



  .container-1 .box-1 .enter-workup-btn:hover {
    background-color: rgb(253, 128, 128);
  }

  .container-1 .box-2 .enter-eval-btn:hover {
    background-color: rgb(255, 215, 129);
  }

  .container-1 .box-3 .enter-comp-btn:hover {
    background-color: rgb(101, 114, 238);
  }

  .container-1 .box-4 .enter-procedure-btn:hover {
    background-color: rgb(118, 202, 137);
  }

</style>
