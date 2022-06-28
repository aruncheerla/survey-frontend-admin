<template>
    <v-app>
      <v-app-bar  >
            <v-img
                class="mx-2"
                :src="logo"
                max-height="40"
                max-width="40"
                contain
            ></v-img>
            <v-app-bar-title  >My Surveys</v-app-bar-title>
            <v-spacer></v-spacer>
            <v-toolbar-items>
              <v-btn 
                    variant="text"
                    @click="goHome" >
                  Home
                </v-btn>
                <v-btn 
                    variant="text"
                    @click="goList"
                    >
                  List
                </v-btn>
                <v-btn 
                    variant="text"
                    @click="goAdd"
                    >
                  Add
                </v-btn>
                <v-btn 
                    variant="text"
                    @click="goLogin"
                    >
                  Login
                </v-btn>
                <v-btn 
                    variant="text"
                    @click="goRegister"
                    >
                  Register
                </v-btn>
            </v-toolbar-items>
      </v-app-bar>
      <v-main >
        <v-container>
          <div id="nav">
            <router-link v-if="authenticated" to="/login" v-on:click.native="logout()" replace>Logout</router-link>
        </div>
          <router-view @authenticated="setAuthenticated" />
        </v-container>
      </v-main>  

      <div class="footer">
           <div class="container">
             <p>© Copyright 2022 My Surveys. All rights reserved</p>
           </div>   
      </div>

    </v-app>
</template>

<script>
import logo from './assets/oc-logo-white.png'
export default {
  name: 'App',

  data: () => ({
    logo,

    return: {
                authenticated: false,
                mockAccount: {
                    username: "bharath123",
                    password: "test"
                }
            }

  }),

  mounted() {
            if(!this.authenticated) {
                this.$router.replace({ name: "login" });
            }
        },

  methods: {
    goHome() {
      this.$router.push({ name: 'home' });
    },
    goAdd() {
      this.$router.push({ name: 'add' });
    },
    goList() {
      this.$router.push({ name: 'tutorials' });
    },
    goLogin() {
      this.$router.push({ name: 'login' });
    },
    goRegister() {
      this.$router.push({ name: 'register' });
    },
    setAuthenticated(status) {
                this.authenticated = status;
            },
            logout() {
                this.authenticated = false;
            }

  },

}
</script>


<style>

.footer{
      border-top: 1px solid #ccc;
}
.footer p{
  text-align: center;
    color: #000;
    font-size: 14px;
    padding: 7px;
}

</style>