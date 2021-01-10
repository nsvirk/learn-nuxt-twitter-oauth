<template>
  <div class="section">
    <div class="container">
      <div class="columns is-centered">
        <div class="column is-6-tablet is-6-desktop is-6-widescreen">
          <div class="control has-icons-left">
            <h1 class="title">
              <fa :icon="['fab', 'twitter']" />
              Twitter Authentication
            </h1>
          </div>
          <h2 class="subtitle">Demo on using Twitter Oauth 1.0a</h2>
          <hr />
          <form action="">
            <div class="field">
              <button
                class="button is-success is-fullwidth is-medium"
                @click.prevent="signInWithTwitterPopup"
              >
                <span>Sign In with Twitter (Popup)</span>
              </button>
            </div>
            <div class="field">
              <button
                class="button is-success is-fullwidth is-medium"
                @click.prevent="signInWithTwitterRedirect"
              >
                <span>Sign In with Twitter (Redirect)</span>
              </button>
            </div>
          </form>
          <hr />
          <section class="section">
            <div class="container">
              <h1 class="title">Authentication Results</h1>
              <h2 class="subtitle">
                <hr />
                <strong>{{ authMessage }}</strong>
                <hr />
                {{ authProviderData }}
                <hr />
                {{ apiKey }}
                <p>{{ $config.databaseURL }}</p>
              </h2>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from "@/firebase";

export default {
  data() {
    return {
      authMessage: "...",
      authProviderData: "...",
      apiKey: "...",
    };
  },
  methods: {
    signInWithTwitterPopup() {
      this.authMessage = "Signing in user...";
      this.authProviderData = "";
      var provider = new firebase.auth.TwitterAuthProvider();

      firebase
        .auth()
        .signInWithPopup(provider)
        .then((result) => {
          var credential = result.credential;

          // This gives you a the Twitter OAuth 1.0 Access Token and Secret.
          // You can use these server side with your app's credentials to access the Twitter API.
          var token = credential.accessToken;
          var secret = credential.secret;

          // The signed-in user info.
          var user = result.user;
          console.log("Signed in with " + user.providerData[0].providerId);
          this.authMessage =
            "Signed in with " + user.providerData[0].providerId;
          this.authProviderData = user.providerData[0];
        })
        .catch((error) => {
          // Handle Errors here.
          var errorCode = error.code;
          var errorMessage = error.message;
          // The email of the user's account used.
          var email = error.email;
          // The firebase.auth.AuthCredential type that was used.
          var credential = error.credential;
          // ...
          alert("Twitter signin error : " + errorMessage);
        });
    },

    signInWithTwitterRedirect() {
      var provider = new firebase.auth.TwitterAuthProvider();
      firebase.auth().signInWithRedirect(provider);
      this.authMessage = "Signing in user...";
      this.authProviderData = "";
    },
  },

  mounted() {
    firebase
      .auth()
      .getRedirectResult()
      .then((result) => {
        if (result.credential) {
          var credential = result.credential;

          // This gives you a the Twitter OAuth 1.0 Access Token and Secret.
          // You can use these server side with your app's credentials to access the Twitter API.
          var token = credential.accessToken;
          var secret = credential.secret;
          // ...
          // The signed-in user info.
          var user = result.user;
          console.log("Signed in with " + user.providerData[0].providerId);
          this.authMessage =
            "Signed in with " + user.providerData[0].providerId;
          this.authProviderData = user.providerData[0];
        }
      })
      .catch((error) => {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;
        // ...
        alert("Twitter signin error : " + errorMessage);
      });
    // [END auth_twitter_signin_redirect_result]
  },
};
</script>


