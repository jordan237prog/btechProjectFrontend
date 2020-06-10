<template>
  <div class="col-md-12">
    <div class="card card-container">

        <input type="file"
        @change="onFileSelected"
        style="display: none"
        ref="fileInput">

      <img
        id="profile-img"
        src="../assets/signup.png"
        class="profile-img-card"
        @click="$refs.fileInput.click()"
        
      /> 
      <form name="form" @submit.prevent="handleRegister">
        <div v-if="!successful">
          <div class="form-group">
            <!-- <label for="username">Username</label> -->
            <input
              v-model="user.username"
              v-validate="'required|min:3|max:20'"
              type="text"
              class="form-control"
              name="username"
              placeholder="Username"
            />
            <div
              v-if="submitted && errors.has('username')"
              class="alert-danger"
            >{{errors.first('username')}}</div>
          </div>
          <div class="form-group">
            <!-- <label for="email">Email</label> -->
            <input
              v-model="user.email"
              v-validate="'required|email|max:50'"
              type="email"
              class="form-control"
              name="email"
              placeholder="Email"
            />
            <div
              v-if="submitted && errors.has('email')"
              class="alert-danger"
            >{{errors.first('email')}}</div>
          </div>
          <div class="form-group">
            <!-- <label for="password">Password</label> -->
            <input
              v-model="user.password"
              v-validate="'required|min:6|max:40'"
              type="password"
              class="form-control"
              name="password"
              placeholder="Password"
              autocomplete="off"
            />
            <div
              v-if="submitted && errors.has('password')"
              class="alert-danger"
            >{{errors.first('password')}}</div>
          </div>

          <div class="form-group">
            <!-- <label for="Vpassword">Verify Password</label> -->
            <input
              v-model="Vpassword"
              v-validate="'required|min:6|max:40'"
              type="password"
              class="form-control"
              name="Vpassword"
              placeholder="Verify Password"
              autocomplete="off"
            />
            <div
              v-if="submitted && errors.has('Vpassword')"
              class="alert-danger"
            >{{errors.first('Vpassword')}}</div>
          </div>

          <div class="form-group">

                <label for="roles">I am a/an </label>

                <select
                 name="roles" id="roles" 
                 class="form-control"
                 placeholder=""
                 v-model="user.roles">
                    
                    <option value="Lance">Lance</option>
                    <option value="Enterprise">Enterprise</option>

                </select>
          </div>


          <div class="form-group">
            <button class="btn btn-block">Sign Up</button>
          </div>
        </div>
      </form>

      <div
        v-if="message"
        class="alert"
        :class="successful ? 'alert-success' : 'alert-danger'"
      >{{message}}</div>
    </div>
  </div>
</template>

<script>
import User from '../models/user';

export default {
  name: 'Register',
  data() {
    return {
      user: new User('', '', '', ''),
      submitted: false,
      successful: false,
      message: '',
      Vpassword:'',
      selectedFile: null
    };
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn;
    }
  },
  mounted() {
    if (this.loggedIn) {
      this.$router.push('/profile');
    }
  },
  methods: {
    handleRegister() {
      this.message = '';
      this.submitted = true;
      this.$validator.validate().then(isValid => {
        if (isValid) {

            if(this.Vpassword === this.user.password){

                this.$store.dispatch('register', this.user).then(
                data => {
                this.message = data.message;
                this.successful = true;
                },
                error => {
                this.message =
                    (error.response && error.response.data.message ) ||
                    error.message ||
                    error.toString();
                this.successful = false;
                }
              );

            }else{this.message = "the password DON'T MATCHE"}
          
        }
      });
    },
    
    //============profil picture upload here ===============================
    onFileSelected(event){

        this.selectedFile = event.target.files[0] 

    },
    onUpload(){



    }
  }
};
</script>

<style scoped>
label {
  display: block;
  margin-top: 10px;
}

.card-container.card {
  max-width: 350px !important;
  padding: 40px 40px;
}

.card {
  background-color: #f7f7f7;
  padding: 20px 25px 30px;
  margin: 0 auto 25px;
  margin-top: 50px;
  -moz-border-radius: 2px;
  -webkit-border-radius: 2px;
  border-radius: 2px;
  -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
}

.profile-img-card {
  width: 50px;
  height: 50px;
  margin: 0 auto 10px;
  display: block;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
}
input{
    height: 35px;
    margin-top: 20px ;
    margin-bottom: 20px ;
}
input:last{
    margin-bottom: 0px;
}
select{
    height: 35px;
}
.btn{
  border: 1.2px solid #8b91dd;
  color: #8b91dd;
}
.btn:hover{
  border: 1.2px solid #8b91dd;
  background-color: #8b91dd;
  color: #ffffff;
}
label{
    color: gray;
}
</style>