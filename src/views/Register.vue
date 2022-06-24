<template>
<h1>Register an Account</h1>
   <v-app id="inspire">
      <v-content>
         <v-container fluid fill-height>
            <v-layout align-center justify-center>
               <v-flex xs12 sm8 md4>
                  <v-card class="elevation-12">
                     <v-toolbar dark color="primary">
                        <v-toolbar-title>Fill the form to Register</v-toolbar-title>
                     </v-toolbar>
                     <v-card-text>
                        <v-form>
                           <v-text-field
                              name="name"
                              label="Full Name"
                              type="text"
                           ></v-text-field>
                           <v-text-field
                              id="password"
                              name="password"
                              label="Password"
                              type="password"
                           ></v-text-field>
                           <v-text-field
                              id="password"
                              name="cpassword"
                              label="Confirm Password"
                              type="password"
                           ></v-text-field>
                           <v-text-field
                              name="email"
                              label="Email"
                              type="text"
                           ></v-text-field>
                           <v-text-field
                              name="dob"
                              label="Date Of Birth (mm/dd/yy)"
                              type="text"
                           ></v-text-field>
                        </v-form>
                     </v-card-text>
                     <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="primary" to="/">Register</v-btn>
                     </v-card-actions>
                  </v-card>
               </v-flex>
            </v-layout>
         </v-container>
      </v-content>
   </v-app>
</template>

<script>
export default {
   name: 'Login',
   props: {
      source: String,
   },
};
</script>

<style>

#inspire .v-layout{
    display: unset;
}
.elevation-12{
    width: 540px;
    margin: 18px auto;
}
.v-card .v-card-text .v-form{
    padding: 18px 10px 0;
}
.v-card-actions {
    text-align: center;
    display: block !important;
}
.v-card-actions .v-btn {
    border: 2px solid;
    margin-bottom: 27px;
    font-size: 18px;
    border-radius: 0;
    padding: 7px 18px;
    height: unset;
}
</style>
