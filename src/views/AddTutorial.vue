<template>
    <h1>Add Survey</h1>
    <h4>{{ message }}</h4>
    <v-form>
       <v-text-field
            label="Survey Name"
            v-model="tutorial.title"
        />
        <v-text-field
            label="Survey Description"
            v-model="tutorial.description"
        />
        <v-select
          :items="items"
          filled
          label="Survey Type"
        ></v-select>

        <v-row justify="center">
            <v-col col="2"> </v-col>
            <v-col col="2">
                <v-btn color="success" @click="saveTutorial()"
                    >Save</v-btn >
            </v-col>
            <v-col col="2">
                <v-btn color="info" @click="cancel()">Cancel</v-btn>
            </v-col>
            <v-col col="2"> </v-col>
        </v-row>
    </v-form>
</template>



<script>
import TutorialDataService from "../services/TutorialDataService";
export default {
  name: "add-tutorial",
  data() {
    return {
      tutorial: {
        id: null,
        title: "",
        description: "",
        published: false
      },
       items: ['Market Research Survey', 'Customer Feedback Survey', 'Product Feedback Survey'],
      message: "Enter data and click save"
    };
  },
  methods: {
    saveTutorial() {
      var data = {
        title: this.tutorial.title,
        description: this.tutorial.description
      };
      TutorialDataService.create(data)
        .then(response => {
          this.tutorial.id = response.data.id;
          console.log("add "+response.data);
          this.$router.push({ name: 'tutorials' });
        })
        .catch(e => {
          this.message = e.response.data.message;
        });
    },
    cancel(){
        this.$router.push({ name: 'tutorials' });
    }
  }
}

</script>
<style>
.v-main__wrap h4{
  margin-bottom: 18px;
}
form button.v-btn {
border: 2px solid;
    margin-bottom: 27px;
    font-size: 18px;
    border-radius: 0;
    padding: 7px 18px;
    color: #000 !important;
    height: unset;
    background-color: #fff !important;
}


</style>