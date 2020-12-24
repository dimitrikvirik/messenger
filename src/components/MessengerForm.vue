<template>
  <div class="msgForm">
    <div class="messages">
      <div v-for="msg in messages" v-bind:key="msg">
        <MessageTo  v-if="checkUser(msg.name)"  :msg="msg.text"  :author="msg.name" :date="msg.createdDate"></MessageTo>
        <MessageFrom v-else  :msg="msg.text" :author="msg.name" :date="msg.createdDate"></MessageFrom>
      </div>
    </div>
    <MessageSend :key="user" :user="user"></MessageSend>
    </div>
</template>

<script>
import MessageFrom from "@/components/MessageFrom";
import MessageTo from "@/components/MessageTo";
import MessageSend from "@/components/MessageSend";
import axios from "axios";

export default {
name: "MessengerForm",
  components: {MessageSend, MessageTo, MessageFrom},
  data(){
    return{
      messages: [],
      msgId: 1,
      user: 'dito',
      myMsg: '',
      value: ''
    }
  },
  comments: [
    MessageFrom,
    MessageTo
  ],
  methods: {
    getMsg: function (){
      const vm = this;
      axios.get('http://localhost:80/server.php',{
         params: {
           msgId: vm.msgId
         }
      }).then(function (response){
        let data = response.data;
        console.log(data);
        if(data) {
          let content = {
            text: response.data.msg,
            name: response.data.name,
            createdDate: response.data.createdDate,
            id: vm.msgId
          };
          vm.msgId++;
          vm.messages.push(content);

        }
      }).catch(function (error){
        console.log(error)
      });
    },
   setUser(){
      let username = prompt("Enter username: ");
      this.user = username;
   },
    checkUser(name){
      return name === this.user;
    }
  },

  created() {
  const vm  = this;
  setInterval(function (){vm.getMsg()}, 400);
  this.setUser();
  }
}

</script>

<style scoped>
  .messages{
    overflow-x: hidden;
    overflow-y: auto;
    height: 100%;
    width:  100%;
  }
  .msgForm{
    width: 800px;
    height: 750px;
    background-color: lightcyan;
  }

</style>