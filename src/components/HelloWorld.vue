<template>
  <div class="hello">
    <h1>{{ message }}</h1>
    <h3>Chat</h3>
    <div class="chat-window" id="chatW">
      <div v-for="(m, idx) in msgs" :key="idx">
        <div class="user-msg">
          <span> {{ m.msg }} </span><span class="user-sgn"> - User</span><br />
        </div>
        <div class="bot-msg">
          <span> {{ m.bMsg }} </span><span class="bot-sgn"> - Bot</span><br />
        </div>
      </div>
    </div>
    <div class="flex-container">
      <input class="msg-box" type="text" placeholder="Message" v-model="txtInput" v-on:keyup.enter="sendMsg" />
      <button class="send-btn" v-on:click="sendMsg">Send</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    message: String
  },
  data: function () {
    return {
      msgs: [],
      txtInput: '',
      botText: ''
    }
  },
  mounted() {
      this.$nextTick(() => {
        var element = document.getElementById("chatW");
        element.scrollTop = element.scrollHeight;
      });
  },
  methods: {
    updateScroll(){
      var element = document.getElementById("chatW");
      element.scrollTop = element.scrollHeight;
    },
    sendMsg(){
      console.log("Msg sent");
      //this.msgs.push({msg: this.txtInput});

      fetch("https://ServerToFetchBotMsgs"+this.txtInput, {
        "method": "GET"
      })
      .then(response => {
        if(response.ok){
          return response.json()   
        } else{
          console.log("Server returned " + response.status + " : " + response.statusText);
        }                
      })
      .then(response => {
        this.result = response;
        console.log(this.result);
        this.botText = this.result.messages[0].text;
        this.msgs.push({msg: this.txtInput, bMsg: this.botText});
      })
      .catch(err => {
          console.log(err);
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 10px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.chat-window {
  height:350px;
  width: 350px;
  overflow: auto;
  margin: 5px auto 10px auto;
  border: 1px solid #aa9f9f;
  border-radius: 5px;
}
.user-msg {
  text-align: right;
  margin: 5px 10px 0 0;
}
.bot-msg {
  text-align: left;
  margin: 5px 0 0 10px;
}
.user-sgn, .bot-sgn {
  font-size: 12px;
  color: rgb(58, 58, 192);
}
.flex-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.msg-box {
  width: 285px;
  padding: 5px;
  margin-right: 5px;
  border-radius: 5px;
  align-self: flex-start;
}
.send-btn {
  padding: 5px;
  border-radius: 5px;
  align-self: flex-end;
}
</style>
