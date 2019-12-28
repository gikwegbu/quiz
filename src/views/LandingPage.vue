<template>
  <div class="landingPage">
     <b-container fluid>
          <div class="header text-white">
          <h2> Welcome To Quizy App</h2>
          <span>This app provides an efficient way to test Job seekers on an Evaluation test to acertain their IQ and EQ levels before employment. </span>
          <br>
          <i>Easy,Fast and Reliable.</i>
      </div>
     </b-container>
     <hr style="background-color: white; height: 1px;">
    <b-container>
        <b-row>
            <b-col sm='6' class="icon text-center">
                    <img alt="Logo" src="../assets/icon.png" />
             </b-col>
            <b-col sm='6' class="w-100 form-field"> 
            <br>
            <div>
                <b-jumbotron class="text-center"> 
                    <h4>{{ title }}</h4>
                </b-jumbotron>
            </div>

                <!-- This is the part for the login details -->
                <div class="loginPart" v-show="login">
                    <b-form  >
                        <b-form-group id="input-group-1"  label-for="email" description="we will never share your email with anyone else" >
                            <b-form-input id="email" type="email" required placeholder="Email" /> 
                        </b-form-group>

                        <b-form-group id="input-group-2"  label-for="password">
                            <b-form-input id="password" type="password" required placeholder="Password"/>
                        </b-form-group>

                        <p class="text-center optionBtn" @click="flip('signup')">Not registered? click here to signup</p>
                        <!-- <p class="text-center optionBtn" @click="signup =  ! signup,login = ! login" >Not registered? click here to signup</p> -->

                        <b-form-group id="loinBtn">
                            <b-button type="submit" class="btn form-control"  variant="primary">
                                Login 
                            </b-button>
                        </b-form-group>
                    </b-form>
                </div>

                <!-- This is the part for the signup details  -->
                <div class="signupPart" v-show="signup">
                    <b-form> 
                        <b-form-group id="input-group-3" label-for="name" description="This name will be visible to other uers.">
                            <b-row>

                                <b-col sm="6">
                                    <b-form-input id="firstname" type="text" required placeholder="Firstname"></b-form-input>
                                </b-col>

                                <b-col sm="6">
                                    <b-form-input id="lastname" type="text" required placeholder="Lastname"></b-form-input>
                                </b-col>

                            </b-row>
                        </b-form-group> 

                        <b-form-group id="input-group-4" label-for="email" label="Email: ">
                            <b-form-input id="email" type="email" required placeholder="e.g g.ikwegbu@gmail.com"></b-form-input>
                        </b-form-group>

                        <b-form-group id="input-group-5" label-for="password" label="Password: " description="This should contain a mixture of letters and numbers">
                           <b-row>
                               <b-col sm="6">
                                    <b-form-input id="password" type="password" required placeholder="e.g holiday12@87!"></b-form-input>
                               </b-col>
                               <b-col sm="6">
                                    <b-form-input id="retypePassword" type="password" required placeholder="Re-type password"></b-form-input>
                               </b-col>
                           </b-row>
                           
                        </b-form-group>

                        <b-form-group id="input-group-6" label-for="retypePassword" label="Re-type Password: " description="This is case sensitive and should match the above.">
                           
                        </b-form-group>

                        <b-form-group class="text-center optionBtn">
                            <p @click="flip">Already have an account? click here to Login</p>
                        </b-form-group>

                        <b-form-group>
                            <b-button type="submit" class="btn b  form-control" variant="primary">
                                SIGN UP
                            </b-button>
                        </b-form-group> 
                    </b-form>
                </div>
             </b-col> 
        </b-row>
    </b-container>
  </div>
</template>


<style  scoped> 
.landingPage{
    background-image:url('../assets/background.jpg');
    background-position: center;
    background-size: cover;
    height: 100vh;
}
.header{
    padding-top: 10px;
}
.icon{ 
    margin-top: 10px;
}
.icon img{
    width: 100%; 
}
.form-field{ 
    /* margin-top: 10px; */
    border-radius: 5px;
    background-color:  white;
}
input{
    border-radius: 20px;
}
.optionBtn{
    cursor: pointer;
    color: rgb(22, 22, 224);
}
</style>

<script>
// @ is an alias to /src 

export default {
  name: "landingPage",
  data(){
      return{
          title: 'LOGIN',
          login: true,
          signup: false
      }
  }, 
  methods: {
      flip(){
         setTimeout(() => {
             if(this.title == "LOGIN"){
                this.title = "SIGN UP"
            }else if(this.title == "SIGN UP"){
                this.title = "LOGIN"
            } 
            this.login = ! this.login;
            this.signup = ! this.signup; 
         }, 1000);
      }, 
  }
};
</script> 
