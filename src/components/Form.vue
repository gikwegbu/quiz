<template>
    <div>
        <div class="text-center">
          <!-- This is where the question settings occurs -->
        <b-jumbotron header="Question Settings" v-if="setQuestion">
          <template slot="lead">
            <i>Fill the below form to Organize your question set</i>
            <hr>
            <b-form @submit.prevent="submit()" class="text-left">
              <b-form-group label="Number of Questions: " label-for="questions">
                <b-form-input type="number" v-model="amount"></b-form-input>
              </b-form-group>

              <b-form-group label="Select Category: " label-for="category">
               <b-form-select id="category" v-model="cat" :options="category"></b-form-select>  
              </b-form-group>

              <b-form-group label="Select Difficulty Mode:" label-for="mode">
                <b-form-select id="mode" v-model="mode" :options="difficulty"></b-form-select> 
              </b-form-group>  

              <hr class="my-4">
              <b-button type='submit' class="btn form-control" variant="primary">
                {{ SubmitMsg }}
              </b-button>

            </b-form>

            
          </template> 
        </b-jumbotron>

        <!-- This is the question proper -->
        <b-jumbotron header="Quiz" v-if='getQuestion'>
          <template slot="lead"> 
            <i>Count: {{count + 1}} / {{questions.length}}</i>
            <br>
            <b>Score: {{ score }} </b>
            <hr>
            <div class="text-center"> 
              <!-- Due to the jsonData received comes with some html code, i rendered it inside the v-html to properly format it -->
              <p  v-html="questions[count].question"></p>
              <hr style="width: 30%">
              <div class="text-left">
                <ul class="options"> 
                  <b-list-group>
                    <b-list-group-item 
                            v-for="(item, index) in shuffledAnswers" 
                            :key="index" 
                            v-html="item"
                            @click.prevent="selectedAns(index)"
                            :class=" findAnswer(index)"
                            :disabled='catchThief'
                      >
                      </b-list-group-item>
                  </b-list-group>
                </ul> 
              </div>
              <div class="actionBtn">
                <b-button 
                  type="button" 
                  @click="submitAns"  
                  :disabled="selectedIndex === null || answered"
                  variant="success"
                >
                  Submit
                </b-button>
 
                 <b-button type="button" @click="nextQuestion" v-show="nextBtn"   variant="primary">
                  Next
                </b-button>
              </div>
            </div>
          </template> 
        </b-jumbotron>
      </div>
    </div>
</template>

<style scoped>
.options{
  padding-left: 0px;
}
.options .list-group{
  margin-left: -1opx !important;

}
.options .list-group-item{
  margin-bottom: 5px; 
  cursor: pointer;
  transition: all 0.2s ease-in;
}
.options .list-group-item:hover{
  background-color: #ddd;
  box-shadow: 1px 1px 1px black;
  transition: all 0.1s ease-in;
  border: 0px thin transparent;
}
.actionBtn button{
 margin: 0 10px;
 padding: 1em 1.5em;
 border-radius: 100px;
 box-shadow: 2px 2px 2px 2px rgb(37, 36, 36);
 transition: all 0.3s ease-in;
}
.actionBtn button:hover{
  box-shadow:0px 0px 0px 0px  rgb(37, 36, 36);
 transition: all 0.1s ease-in;
}
.selected{
  background-color: rgb(62, 62, 231);
  color: white;
}
.correct{
  background-color: rgb(22, 175, 22);
  color: white;
}
.wrong{
  background-color: rgb(204, 35, 35);
  color: white;
}
</style>

<script>
import _ from 'lodash'

  export default {
    data() {
      return { 
        amount: 10,
        cat: null,
        mode: null, 
        response: null,
        category: [
          { value: null, text: 'Please select an option' },
          { value: 'any', text: 'Any Category' },
          { value: '9', text: "General Knowledge" },
          { value: '10', text: 'Entertainment:  Books'},
          { value: '11', text: 'Entertainment:  Film'},
          { value: '12', text: 'Entertainment:  Music'},
          { value: '14', text: 'Entertainment:  Television'},
          { value: '15', text: 'Entertainment:  Video Games'},
          { value: '16', text: 'Entertainment:  Board Games'}, 
          { value: '29', text: 'Entertainment:  Commics'}, 
          { value: '32', text: 'Entertainment:  Cartoon & Animations'}, 
          { value: '17', text: 'Science & Nature'},
          { value: '18', text: 'Science: Computer'},
          { value: '19', text: 'Science: Mathematics'},
          { value: '30', text: 'Science: Gadgets'},
          { value: '20', text: 'Mytholgy'},
          { value: '21', text: 'Sports'},
          { value: '22', text: 'Geography'},
          { value: '23', text: 'History'},
          { value: '24', text: 'Politics'},
          { value: '25', text: 'Art'},
          { value: '26', text: 'Celebrities'},
          { value: '27', text: 'Animal'},
          { value: '28', text: 'Vehicles'}, 
        ], 
         difficulty: [
          { value: null, text: "Please select Response Type"},
          { value: 'any', text: 'Any Type'},
          { value: 'easy', text: 'Easy'},
          { value: 'medium', text: 'Medium'},
          { value: 'hard', text: 'Hard'},
        ], 
        questions: [],
        count: 0,
        SubmitMsg : "Submit",
        setQuestion: true,
        getQuestion: false, 
        selectedIndex: null,
        shuffledAnswers: [],
        correctIndex: null,
        nextBtn: true,
        score: 0,
        catchThief: false,
        answered: false
      }
    }, 
    methods: {
      submit: function(){
        let _ = this;
        _.SubmitMsg = "Formulating..."
       fetch("https://opentdb.com/api.php?amount="+this.amount+"&category="+this.cat+"&difficulty="+this.mode+"&type=multiple ", {
         method: 'get'
       }).then((response) => {
         return response.json();
       }).then((jsonData) => {
         _.questions = jsonData.results;
         _.setQuestion = false;
         _.getQuestion = true;  
        _.shuffledAns();
       })
      },
      selectedAns(index){
        this.selectedIndex = index; 
         
      },
      submitAns: function(){
        this.catchThief = true;
         if(this.selectedIndex == this.correctIndex){ 
          this.score++;
        }
        this.answered = true; 
      },
      nextQuestion: function(){
        this.count++;
        this.selectedIndex = null;
         this.answered = false; 
        this.catchThief = false;
        this.shuffledAns(); 
        if(this.count == this.questions.length-1){ 
          this.nextBtn = false
        }
       
      },
      shuffledAns: function(){
        let answers = [...this.questions[this.count].incorrect_answers, this.questions[this.count].correct_answer];
        this.shuffledAnswers = _.shuffle(answers); 
        this.correctIndex = this.shuffledAnswers.indexOf(this.questions[this.count].correct_answer); 
      },
      findAnswer: function(index){
        if(!this.answered && this.selectedIndex === index){
          return 'selected'
        }else if(this.answered && this.correctIndex === index){
          return 'correct'
        }else if(this.answered && this.selectedIndex != this.correctIndex && this.selectedIndex === index){
          return 'wrong'
        }
      }
    }, 
    computed: {  
      
    }
  };

</script>