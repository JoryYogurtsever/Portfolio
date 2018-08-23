<template>
  <div id="contactArea" @click.prevent="resetEntries($event)">
    <div id="youMad" class="sneaky">
     <img class="SadImage" src="../../static/AreYouMad.png">
    </div>
    <div id="noEmail" class="sneaky">
      <img class="SadImage" src="../../static/emailYou.png">
    </div>
    <div id="noName" class="sneaky">
      <img class="SadImage" src="../../static/yourname.png">
    </div>
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
      resetEntries(event) {
        {
          if (event.target.id === 'name' || event.target.id === 'email' || event.target.id === 'phone' || event.target.id === 'message') {
          } else {
            if (this.userMessage.name === '') {
              this.userMessage.name = 'name';
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
          }
        }
      },
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
        if (this.userMessage.name === ' ' || this.userMessage.name === 'name') {
          document.getElementById('noName').removeAttribute('class');
          setTimeout(function() {
            document.getElementById('noName').setAttribute('class', 'sneaky');
          }, 2500);
        } else if (this.userMessage.email === ' ' || this.userMessage.email === 'email') {
          document.getElementById('noEmail').removeAttribute('class');
          setTimeout(function() {
            document.getElementById('noEmail').setAttribute('class', 'sneaky');
          }, 3500);
        } else if (this.userMessage.message === ' ' || this.userMessage.message === 'message') {
          document.getElementById('youMad').removeAttribute('class');
          setTimeout(function() {
            document.getElementById('youMad').setAttribute('class', 'sneaky');
          }, 5000);
        } else {
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
  }
</script>

<style>
  #contactArea {
    background-image: url("../../static/PixelUs.png");
    height: 650px;
    /*background-attachment: fixed;*/
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;;
    width: 100%;
    position: relative;
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
  .SadImage {
    position: absolute;
    left: 50%;
    top: 20%;
    z-index: 2;
    width: 200px;
  }
  .sneaky {
    display: none;
  }
  @media (max-width:640px) {
    #contactArea {
      background-image: url("../../static/PixelUs.png");
      height: 370px;
    }
    .formItem {
      font-size: 0.6em;
      margin-left: 5%;
      padding-left: 5%;
      z-index: 2;
      width: 130px;
      height:40px;
      border-radius: 10px;
      margin-top: 10px;
    }
    .textArea {
      font-size: 0.6em;
      width: 75%;
      height: 90px;
      margin-top: 108px;
      margin-left: 20px;
      padding-left: 50px;
      padding-top: 20px;
    }
    .formButton {
      font-size: 0.6em;
      width: 60px;
      height: 20px;
      margin-left: 20px;
      border: none;
    }
    .SadImage {
      position: absolute;
      left: 50%;
      top: 20%;
      z-index: 2;
      width: 120px;
    }
  }
</style>
