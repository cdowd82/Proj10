<script>
    import { FirebaseApp, User, Doc, Collection } from "sveltefire";

    import firebase from "firebase/app";
    import "firebase/firestore";
    import "firebase/auth";
    import "firebase/performance";
    import "firebase/analytics";

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

    function signIn() {
        firebase.auth().signInWithEmailAndPassword(email, password).then((user) => {
            // Signed in 
            // ...
        })
        .catch((error) => {
            var errorCode = error.code;
            var errorMessage = error.message;
        });
    }

    function signOut() {
        firebase.auth().signOut().then(() => {
            // Sign-out successful.
        }).catch((error) => {
            // An error happened.
    });

    }

    firebase.initializeApp(firebaseConfig);
</script>

<main>

  {#if !firebaseConfig.projectId}
    <strong>Step 0</strong>
    Create a
    <a href="https://firebase.google.com/">Firebase Project</a>
    and paste your web config into
    <code>App.svelte</code>
    .
  {/if}

  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>

    <h1>Liverpool TAViTRAK</h1>

    <!-- 2. 😀 Get the current user -->
    <User let:user let:auth>
      Signed in as: 
      <em>{user.email}</em>

      <button on:click={() => auth.signOut()}>Sign Out</button>

      <div slot="signed-out">

            <input type="text" bind:value={email} placeholder="Email">
            <br>
            <input type="password" bind:value={password} placeholder="Password">
            <br>
            <button on:click={signIn}>Login</button>

        <!--
        <button on:click={() => auth.signInAnonymously()}>
          Sign In Anonymously
        </button>
        -->
      </div>

      <hr />

      <!-- 3. 📜 Get a Firestore document owned by a user -->
      <Doc path={`posts/${user.uid}`} let:data={post} let:ref={postRef} log>

        <h2>{post.title}</h2>

        <p>
          Document
          created at <em>{new Date(post.createdAt).toLocaleString()}</em>
        </p>

        <span slot="loading">Loading post...</span>
        <span slot="fallback">
          <button
            on:click={() => postRef.set({
                title: '📜 I like Svelte',
                createdAt: Date.now()
              })}>
            Create Document
          </button>
        </span>

        <!-- 4. 💬 Get all the comments in its subcollection -->

        <h3>Comments</h3>
        <Collection
          path={postRef.collection('comments')}
          query={ref => ref.orderBy('createdAt')}
          let:data={comments}
          let:ref={commentsRef}
          log>

          {#if !comments.length}
              No comments yet...
          {/if}

          {#each comments as comment}
            <p>
              <!-- ID: <em>{comment.ref.id}</em> -->
            </p>
            <p>
              {comment.text}
              <button on:click={() => comment.ref.delete()}>Delete</button>
            </p>
          {/each}


          <button
            on:click={() => commentsRef.add({
                text: '💬 Me too!',
                createdAt: Date.now()
              })}>
            Add Comment
          </button>

          <span slot="loading">Loading comments...</span>

        </Collection>
      </Doc>
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