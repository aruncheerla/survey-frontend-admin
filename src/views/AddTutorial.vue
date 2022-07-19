<template>
<h1>Add Survey</h1>
<h4>{{ message }}</h4>
<v-form>
  <v-text-field label="Survey Name" v-model="tutorial.title" />
  <v-text-field label="Survey Description" v-model="tutorial.description" />
  <v-select :items="items" filled label="Survey Type" v-model="tutorial.type"></v-select>
  <v-text-field
    name="startDate"
    label="Start Date (mm/dd/yy)"
    type="date"
    v-model="startDate"
  ></v-text-field>
  <v-text-field
    name="endDate"
    label="End Date (mm/dd/yy)"
    type="date"
    v-model="endDate"
  ></v-text-field>

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
  <v-card-text v-if="flag==true">

  <v-card-text
   v-for="(textField, i) in textFields"
   :key="i"
  >
    <v-text-field
     :label="textField.label1"
     v-model="textField.value1"
    ></v-text-field>
    <v-select :items="quesarray" filled label="Question Type" v-model="textFields[i].qtype"></v-select>
    <v-btn style="width:100%" color="info" @click="add(i)" v-if="textFields[i].qtype=='mcq' || textFields[i].qtype=='checkbox'">add</v-btn>
    <template v-for="(input, index) in textField.inputs" :key="index"><br>


    <v-row v-if="textFields[i].qtype=='mcq'">
    <v-radio></v-radio><v-text-field v-model="textField.inputs[index]"></v-text-field>&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp
    <v-btn class="remove" onclick="return this.parentNode.remove();">X</v-btn>
    </v-row>

    <v-row v-if="textFields[i].qtype=='checkbox'">
    <v-checkbox></v-checkbox><v-text-field v-model="textField.inputs[index]"></v-text-field>&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp&nbsp;&nbsp
    <v-btn class="remove" onclick="return this.parentNode.remove();">X</v-btn>
    </v-row>

    </template><br><br>
    <v-btn @click="removeQ(i)" class="error">DElete</v-btn>
    </v-card-text>






<v-row align="end" justify="end">
    <v-btn @click="addQ" class="primary">add</v-btn>
      <v-btn color="success" @click="saveQuestions()">Save</v-btn>

</v-row>

  </v-card-text>
</v-form>
</template>



<script>
import axios from "axios";
import Datepicker from 'vue3-datepicker'
import moment from 'moment'

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
      quesArray:[],
      quesarray: ['description', 'checkbox', 'mcq'],
      message: "Enter data and click save",
      flag: false,
      active: 1,
      radioGroup: "",
      array: ["test", "tool"],
      qtype: '',
      test: "",
      surveyId: 0,
      qName: '',
      startDate:'',
      endDate:'',
      textFields: [{label1: "Question 1",value1: "",inputs: [],qtype:[]}]
    };
  },
  methods: {
  addQ () {
        this.textFields.push({
          label1: "Question "+(this.textFields.length+1),
          value1: "",
          inputs:[]
        })
     },

     removeQ(index) {
         this.textFields.splice(index, 1)
     },
  momentFunction(date){
    return moment(date).format('DD-MM-YYYY');
  },
    saveQuestions() {
    console.log(this.surveyId)
    var data={};
    let questionDetails=[];
    this.textFields.map(item=>{
    let str=""
      if(item.inputs.length>0){
        item.inputs.map(i=>{
                str+=i+","
        })
      }
      str.slice(0,-1)
      questionDetails.push({surveyQuestionType:item.qtype,
      surveyQuestionText:item.label1,
      surveyQuestionOptionDetails:str})
    })

    data.questionDetails=questionDetails
    data.surveyId=this.surveyId

    console.log(data,"++++++++++++++++pppppppppppppppppppp++++")
    var config = {
      method: 'post',
      url: 'http://localhost:9005/api/surveyQuestion/bulkCreateSurveyQuestion',
      headers: {
        'x-developer-token': 'c256f988-459a-43ca-8fef-9c14f7134900',
        'x-api-key': 'qwrtrthedwd2124@#$%2sSQw2',
        'Content-Type': 'application/json'
      },
      data : JSON.stringify(data)
    };

    axios(config)
    .then(function (response) {
      console.log(JSON.stringify(response.data));
    })
    .catch(function (error) {
      console.log(error);
    });



    },
    add(i) {
      this.textFields[i].inputs.push(this.textFields[i].inputs.length+1)
    },
    saveTutorial() {
    var self=this;
      var data = {
        surveyName: this.tutorial.title,
        surveyIntroParagraph: this.tutorial.type,
        surveyDescription: this.tutorial.description,
        surveyStartDate:this.momentFunction(this.startDate),
        surveyEndDate:this.momentFunction(this.endDate),
        userId:2
      };
      console.log(data)
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
          self.surveyId = response.data.responseData.surveyId
          self.flag = true
        })
        .catch(function(error) {
          console.log(error);
        });
        console.log(this.surveyId,"???????????????????")
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
