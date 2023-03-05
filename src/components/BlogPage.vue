<template>
  <div id="blog-page">
    <div class="left"  v-if="blogmsg != null">
    <div class="card"   v-for="b in blogmsg" :key="b.id">
      <div class="container">
        <h4><b>{{ b.username }}</b></h4> 
        <p>{{b.message}}</p> 
      </div>
    </div>
    </div>
    <div class="left">
    <div class="card">
      <div class="container">
        <h4><b>{{ username }}</b></h4> 
        <textarea v-model="blog.message"></textarea>
         <button @click="postBlog">Post Message</button>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
import SockJS from "sockjs-client";
import Stomp from "webstomp-client";
import axios from 'axios';

export default {
   name: 'BlogPage',
   data() {
      return {
        blogmsg: [],
        blog: {
          username: '',
          message: ''
        },
        stompClient: null
      };
   },
   created(){
      this.username = localStorage.getItem("username");
   },
   mounted(){
    this.connect();
    this.getAllblog();
   },
   methods:{
    connect(){
      let socket = new SockJS("http://localhost:9090/server");
      this.stompClient = Stomp.over(socket);
      this.stompClient.connect(
        {},
        frame =>  {
          console.log("connected", frame);
            this.stompClient.subscribe("/topic/return-to", response => {
              console.log("Connected: ",JSON.parse(response.body));
              this.getAllblog();
              }); 
        },
        error => {
          console.log(error);
        });
    },
    send(){
      let msg = {
          message: "update blog"
      }
      this.stompClient.send("/app/message", JSON.stringify(msg), {});
    },
    getAllblog(){
        axios.get(`http://localhost:9090/getAllBlog`)
    .then(response => {
      console.log(response);
      this.blogmsg = response.data.object;
    })
    .catch(e => {
      console.log(e);
    });
    },
    postBlog(){
      this.blog.username = localStorage.getItem("name")
      axios.post(`http://localhost:9090/createBlog`, this.blog)
    .then(response => {
      console.log(response);
      this.send();
    })
    .catch(e => {
      console.log(e);
    });

    }
   }
}
</script>

<style>
textarea{
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  box-sizing: border-box;
  border-radius: 5px;
}
.left{
  float: left;
  width: 50%;
}
.right {
  float: right;
  width: 50%;
}
.card {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  width: 40%;
}

.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

.container {
  padding: 2px 16px;
}
</style>