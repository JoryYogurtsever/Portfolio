<template>
  <div class="contactArea">
    <!--img class="contactImage" src="../../static/PixelUs.png"-->
    <form class="yogurtForm">
      <input type="text" id="name" @click="resetName()" class="formItem" v-model="userMessage.name">
      <input type="text" id="email" @click="resetEmail()" class="formItem" v-model="userMessage.email">
      <input type="text" id="phone" @click="resetPhone()" class="formItem" v-model="userMessage.phone"><br>
      <div><textarea class="textArea" @click="resetMessage()" :id="userMessage.message" v-model="userMessage.message"></textarea></div>
      <div><button class="formButton" @click="submitForm()">Submit!</button></div>

    </form>
  </div>
</template>

<script>
  export default {
    data: function(){
      return {
        userMessage: {
          name: 'name',
          email: 'email',
          phone: 'phone (optional)',
          message: 'message'
        }
      }
    },
    methods: {
      resetName() {
        if (this.userMessage.name === 'name') {
          this.userMessage.name = '';
        }
        if (this.userMessage.email === '') {
          this.userMessage.email = 'email';
        }
        if (this.userMessage.phone === '') {
          this.userMessage.phone = 'phone (optional)';
        }
        if (this.userMessage.message === '') {
          this.userMessage.message = 'message';
        }
      },
      resetEmail() {
        if (this.userMessage.email === 'email') {
          this.userMessage.email = '';
        }
        if (this.userMessage.name === '') {
          this.userMessage.name = 'name';
        }
        if (this.userMessage.phone === '') {
          this.userMessage.phone = 'phone (optional)';
        }
        if (this.userMessage.message === '') {
          this.userMessage.message = 'message';
        }
      },
      resetPhone() {
        if (this.userMessage.phone === 'phone (optional)') {
          this.userMessage.phone = '';
        }
        if (this.userMessage.email === '') {
          this.userMessage.email = 'email';
        }
        if (this.userMessage.name === '') {
          this.userMessage.name = 'name';
        }
        if (this.userMessage.message === '') {
          this.userMessage.message = 'message';
        }
      },
      resetMessage() {
        if (this.userMessage.message === 'message') {
          this.userMessage.message = '';
        }
        if (this.userMessage.email === '') {
          this.userMessage.email = 'email';
        }
        if (this.userMessage.phone === '') {
          this.userMessage.phone = 'phone (optional)';
        }
        if (this.userMessage.name === '') {
          this.userMessage.name = 'name';
        }
      },
      submitForm() {
        this.$http.post('https://vuejs-http-96a4b.firebaseio.com/formTest.json', this.userMessage)
          .then(() => {
            this.userMessage.name = 'name';
            this.userMessage.email = 'email';
            this.userMessage.phone = 'phone (optional)';
            this.userMessage.message = 'message';

          }, error => {
            console.log(error);
          });
      }
    }
  }
</script>

<style>
  .contactArea {
    background-image: url("../../static/PixelUs.png");
    height: 650px;
    /*background-attachment: fixed;*/
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;;
    width: 100%;
    /*position: relative;*/
  }
  .formItem {
    font-family: 'Cinzel', serif;
    /*float: left;*/
    /*position: absolute;*/
    margin-left: 50px;
    padding-left: 50px;
    z-index: 2;
    width: 230px;
    height:40px;
    border-radius: 10px;
    margin-top: 80px;
  }
  .formItem:hover {
    background-color: gainsboro;
  }
  .textArea:hover {
    background-color: gainsboro;
  }
  .textArea {
    font-family: 'Cinzel', serif;
    /*float: left;*/
    width: 230px;
    height: 150px;
    margin-top: 93px;
    margin-left: 50px;
    border-radius: 10px;
    padding-left: 50px;
    padding-top: 20px;
  }
  .formButton {
    font-family: 'Cinzel', serif;
    /*float: left;*/
    background-color: red;
    width: 80px;
    border-radius: 10px;
    height: 40px;
    margin-left: 600px;
    border: none;
  }
</style>
