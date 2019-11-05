<template>
  <div class="global">
    <div class="result">
      <ul>
        <li v-for="chat in chat" :key="chat.id" :class="{ 'bot': chat.isBot, 'user': !chat.isBot }">
          <p v-if="chat.message">
            <strong v-if="chat.author"> {{ chat.author }} :</strong>
            {{ chat.message }}
          </p>
        </li>
      </ul>
    </div>
    <form action="#" class="form">
        <input type="text" v-model="msg" class="form-search-input" placeholder="Informe sua mensagem">
        <button @click="enviar" class="form-search-button"> Enviar </button>
    </form>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  name: 'Form',
  data(){
    return{
      msg: '',
      chat: [{
        id: null,
        author: null,
        message: null,
        isBot: null,
      }]
    }
  },
  methods:{
    enviar(){

      if(this.msg == "" || !this.msg){
        return false
      }

      let mensagem = this.msg;

      this.msg = "";

      let msg = {
        id: this.chat.length,
        author: "Usuário",
        message: mensagem,
        isBot: false
      }
      
      this.chat.push(msg);

      axios
      .get(`http://127.0.0.1:5000/${mensagem}`)
      .then(response => {

        let bot = {
          id : this.chat.length,
          author : response.data.author,
          message : response.data.message,
          isBot: true
        }

        this.chat.push(bot);
      })
        
    }
  }
}
</script>


<style scoped>


.global{
  width: 600px;
  height: 90vh;
  background-color: #eee;
  text-align: center;
  vertical-align: center;
  margin: 50px auto 0;
  filter: drop-shadow(5px 5px 10px #bbb);
  border-radius: 10px;
}

.form{
  width: 100%;
  height: 60px;
  background-image: linear-gradient(to bottom, #5ec, #0aa) ;
  padding-top: 10px;
  border-radius: 0px 0px 10px 10px;
  bottom: 0;
  position: absolute;
}

.form-search-input{
  width: 50%;
  height: 20px;
  margin: 10px;
  border-radius: 10px;
  border: #fff 2px solid;
  background-color: transparent;
  color: #fff;
  padding: 5px 10px;
  font-size: 16px;
}

.form-search-input::placeholder{
  color: #fff;
}

.form-search-button{
  height: 30px;
  width: 100px;
  background-color: transparent;
  color: #fff;
  border: #fff 2px solid;
  border-radius: 10px;
  font-size: 14px;
}

.result{
  width: 90%;
  padding: 20px 5% 50px 5%;
  max-height: 75vh;
  margin: 0 auto;
  font-family: sans-serif;
  color: #333;
  line-height: 25px;
  overflow-y: auto;
}

.result ul{
  list-style: none;
  margin: 0 auto;
  padding: 0px;
}

.result ul li{
  width: auto;
  max-width: 200px;
  height: auto;
  background-color: #fff;
  padding: 10px;
  filter: drop-shadow(3px 3px 5px #bbb);
  border-radius: 20px;
  margin: 10px
}

.result ul li:first-child{
  display: none;
}

.user{
  color: #f00;
}

.user:after {
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  border-left: 0px solid transparent;
  border-right: 20px solid transparent;
  border-top: 20px solid #fff;
  bottom: -20px;
  left: 20%;
}

.bot{
  color: #00f;
  align-self: right;
  margin-left: 55% !important;
}

.bot:after {
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  border-left: 20px solid transparent;
  border-right: 0px solid transparent;
  border-top: 20px solid #fff;
  bottom: -20px;
  left: 70%;
}



</style>
