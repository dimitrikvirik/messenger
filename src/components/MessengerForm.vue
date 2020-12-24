<template>
  <div class="msgForm">
    <div class="messages">
      <div v-for="msg in messages" v-bind:key="msg.messageId">
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
      axios.get(this.$store.state.request_url + "message")
          .then(function (response){
        let data = response.data;
        console.log(data);
        if(data) {
          data.forEach(function (e){
            //Message Read
            let msgContent = {
              text: e.text,
              userId: e.userId,
              createdDate: e.createDate,
              id: e.messageId,
              name: "test"
            }
            axios.get(vm.$store.state.request_url + "user/"+msgContent.id)
              .then(function (response){
                  //User Read
                let userContent = {
                  firstname: response.data.firstname,
                  lastname: response.data.lastname,
                  nickname: response.data.nickname
                }
                msgContent.name = userContent.nickname
              });
            vm.msgId = msgContent.id;
            vm.messages.push(msgContent);
          })
        }
      }).catch(function (error){
        console.log(error)
      });
    },
   setUser(){
     // let username = prompt("Enter username: ");
      this.user = 'dito';
   },
    checkUser(name){
      return name === this.user;
    }
  },

  created() {
  const vm  = this;
  vm.getMsg();
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