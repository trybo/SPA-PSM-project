<template>
  <div id="login-container" class="overflow-hidden">
    <Navbar />
    <div>
      <div class="container d-flex justify-content-center align-items-center">
        
          <div class="card text-center mt-3" id="login">
            <div class="card-body">
              <div class="h3 text-center">Sign in</div>
              <br />
              <div v-if="error" class="alert alert-danger">{{error}}</div>
              <div class="d-flex flex-column align-items-center">
                <button class="btn bg-primary text-white mb-3" id="btn-fb" @click="facebookLogin">
                  Sign in with Facebook
                </button>
                <button class="btn bg-danger text-white mb-3" id="btn-google" @click="googleLogin">
                  Sign in with Google
                </button>
              </div>

              <div class="h6 text-center mb-3">or</div>
              <form action="#" @submit.prevent="submit">
                <div class="form-group row">
                  <input
                    id="email"
                    type="email"
                    placeholder="Email"
                    class="form-control mx-4"
                    name="email"
                    value
                    required
                    autofocus
                    v-model="form.email"
                  />
                </div>

                <div class="form-group row">
                  <input
                    id="password"
                    placeholder="Password"
                    type="password"
                    class="form-control mx-4"
                    name="password"
                    required
                    v-model="form.password"
                  />
                </div>

                <div class="text-center mb-3">
                  <button type="submit" class="btn">Sign in</button>
                </div>
                <p>
                  Forgot your password? <a href="#" @click="resetPassword">Click here</a>
                </p>
              </form>
            </div>
          </div>
        </div>
      </div>
    
    <Footer />
  </div>
</template>

<script>
import firebase from "firebase";
import { router } from "../main";

export default {
  components: {
    Navbar,
    Footer
  },
  data() {
    return {
      form: {
        email: "",
        password: ""
      },
      error: null
    };
  },

  methods: {
    submit() {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.form.email, this.form.password)
        .then(() => {
          router.push("/profile");
        })

        .catch(err => {
          this.error = err.message;
        });
    },
    resetPassword() {
      var auth = firebase.auth();
      var emailAddress = this.form.email;
      if (emailAddress==''){
        alert('Select email!')
      }
      else{
      auth.sendPasswordResetEmail(emailAddress).then(function() {
        alert('Email sent.')
      }).catch(function(error) {
        console.log(error.message);
      });
      }
    },
    async facebookLogin() {
      try {
        const provider = new firebase.auth.FacebookAuthProvider();
        const result = await firebase.auth().signInWithPopup(provider);
        this.$router.push("/");
        console.log(result.user);
      } catch (error) {
        console.log(error.message);
      }
    },
    async googleLogin() {
      try {
        const provider = new firebase.auth.GoogleAuthProvider();
        const result = await firebase.auth().signInWithPopup(provider);
        this.$router.push("/");
        console.log(result.user);
      } catch (error) {
        console.log(error.message);
      }
    }
  },
  beforeMount() {
          firebase.auth().onAuthStateChanged(function(user) {
          
        if (user) {
          router.push("/profile");
        } 
      });
    
  },
};
import Navbar from "@/components/Navbar";
import Footer from "@/components/Footer";
</script>

<style>
#login-container {
  min-height: 100vh; /* will cover the 100% of viewport */
  overflow: hidden;
 display: block;
  position: relative;
  padding-bottom: 100px;
}
.btn {
  background-color: #003c8f !important;
  color: white !important;
}
#btn-fb, #btn-google{
  width: 200px !important;
}
</style>
