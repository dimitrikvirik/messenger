<template>
  <div class="MessageFrom">
    <p class="authorName">From: {{author}}</p>
    <div class="text"><p>{{msg}}</p></div>
    <p class="date">{{date}}</p>
  </div>
</template>

<script>
export default {
  name: "MessageFrom",
  props: [
      'msg',
      'author',
      'date'
  ]
}
</script>

<style scoped>
.MessageFrom{
  position: relative;
  width: 40%;
  height: 55px;
  margin: 10px;
  border-radius: 5px;
  background-color: lightblue;
}
.MessageFrom .authorName{
  color: rgba(0, 0,0,0.4);
  width: 200px;
  margin-left: -50px;

}
.MessageFrom .text{
  display: flex;
  justify-content: center;
  margin-top: -35px;
}
.MessageFrom .date{
  color: rgba(0, 0,0,0.4);
  width: 200px;
  margin-left: -10px;
  margin-top: -15px;
}
</style>