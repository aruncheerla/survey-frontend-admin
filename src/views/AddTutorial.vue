<template>
<h1>Add Survey</h1>
<h4>{{ message }}</h4>
<v-form>
  <v-text-field label="Survey Name" v-model="tutorial.title" />
  <v-text-field label="Survey Description" v-model="tutorial.description" />
  <v-select :items="items" filled label="Survey Type" v-model="tutorial.type"></v-select>

  <v-row justify="center">
    <v-col col="2"> </v-col>
    <v-col col="2">
      <v-btn color="success" @click="saveTutorial()">Save</v-btn>
    </v-col>
    <v-col col="2">
      <v-btn color="info" @click="cancel()">Cancel</v-btn>
    </v-col>
    <v-col col="2"> </v-col>
  </v-row>
  <v-row v-if="flag==true">
    <v-text-field style="width:100%" label="Question Name" v-model="qName" /><br>
    <v-select :items="quesarray" filled label="Question Type" v-model="qtype"></v-select>
    <v-btn style="width:100%" color="info" @click="add()" v-if="flag==true">add</v-btn>
    <div v-for="(input, index) in inputs"><br>
      <input v-if="qtype=='mcq'" type="radio" class="custom-control-input" name="defaultExampleRadios">
      <input v-if="qtype=='checkbox'" type="checkbox" class="custom-control-input" name="defaultExampleRadios">
      <v-text-field style="width=500px" label="option" Name v-model="inputs[index]" /><br>
      <button class="remove" onclick="return this.parentNode.remove();">X</button>
    </div>
    <v-col col="2">
      <v-btn color="success" @click="saveQuestions()">Save</v-btn>
    </v-col>
  </v-row>
</v-form>
</template>



<script>
import axios from "axios";

export default {
  name: "add-tutorial",
  data() {
    return {
      tutorial: {
        id: null,
        title: "",
        description: "",
        published: false,
      },
      items: ['Market Research Survey', 'Customer Feedback Survey', 'Product Feedback Survey'],
      quesarray: ['description', 'checkbox', 'mcq'],
      message: "Enter data and click save",
      flag: false,
      active: 1,
      radioGroup: "",
      array: ["test", "tool"],
      qtype: '',
      test: "",
      inputs: [],
      surveyId: 0,
      qName: ''
    };
  },
  methods: {
    saveQuestions() {
        let str = ''
      var data = {
        "surveyId": Number(this.surveyId),
        "questionType": this.qtype,
        "questionName": "this.qName"
      };
      if (this.qtype == 'description') {
        data.description = this.qtype
      }
      if (this.qtype == 'mcq' || this.qtype == 'checkbox') {
        this.inputs.map((item, index) => {
          if(this.inputs.length>1){
            str += item+","
          }else{
            str += item
          }
        })
        console.log(str, "strrrrrr")
        data.optionDetails=str
      }


      var config = {
        method: 'post',
        url: 'http://localhost:9005/api/surveyQuestion/createQuestion',
        headers: {
          'x-developer-token': 'c256f988-459a-43ca-8fef-9c14f7134900',
          'x-api-key': 'qwrtrthedwd2124@#$%2sSQw2',
          'Content-Type': 'application/json'
        },
        data: data
      };

      axios(config)
        .then(function(response) {
          console.log(JSON.stringify(response.data));
          alert("QUESTION ADDED SUCCESSFULLY");
        })
        .catch(function(error) {
          console.log(error);
        });


    },
    add() {
      this.inputs.push(this.inputs.length + 1);
    },
    saveTutorial() {
      var data = {
        surveyName: this.tutorial.title,
        surveyType: this.tutorial.type,
        description: this.tutorial.description,
        createdBy: "OK"
      };
      var surveyd;
      var config = {
        method: 'post',
        url: 'http://localhost:9005/api/surveydetails/createSurvey',
        headers: {
          'x-developer-token': 'c256f988-459a-43ca-8fef-9c14f7134900',
          'x-api-key': 'qwrtrthedwd2124@#$%2sSQw2',
          'Content-Type': 'application/json'
        },
        data: data
      };
      axios(config)
        .then(function(response) {
          console.log(JSON.stringify(response.data));
          surveyd = response.data.surveyId
        })
        .catch(function(error) {
          console.log(error);
        });
      this.surveyId = surveyd
      this.flag = true
    },
    cancel() {
      this.$router.push({
        name: 'tutorials'
      });
    }
  }
}
</script>
<style>
.v-main__wrap h4 {
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
