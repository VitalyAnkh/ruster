<template>
  <div id="mnav">
    
      <div id="lnav">
        <div id="line"></div>
        <div id="left">
          <a id="front" href="/"><img src="../../../static/imgs/bjtu-x.jpg"/></a>
          <!-- <a id="name" href="/">Rust</a> -->
          <span id="dlnav">
            <a id="tail" href="https://bjtu.edu.cn" target="_blank">交大主页</a>
            <a id="tail" href="https://mis.bjtu.edu.cn" target="_blank">教务系统</a>
            <!-- <a id="tail" href="">search</a> -->
          </span>
        </div>
        <label ><a href="#" id="menu">M</a></label>
      </div>
      <div id="rnav">
          <div id="mlnav"> 
            <!-- <a id="tail" href="/">首页</a> -->
            <!-- <a id="tail" href="">search</a> -->
          </div>
          <li v-if="signin_user.username"> 
            <a v-if="messages_count != 0" :href="'/a/user/' + signin_user.id + '/message'" id="mnumber">{{messages_count}}</a>
          </li>
          <li v-if="signin_user.username == 'admin'"> 
            <a href="/a/create" title="create">新建</a>
          </li>
          <li v-if="signin_user.username"> 
            <a href="/a/post" title="post">发布</a>
            <a :href="'/a/user/' + signin_user.id" title="signin_userusername">{{signin_user.username}}</a>
            <a href="/a/signin" title="Logout" @click="logout">退出</a>
            <a href="/a/help" title="signin">捐助</a>
          </li>
          <li v-else > 
            <a href="/a/signin" title="signin">登录</a>
            <a href="/a/signup" title="signin">注册</a>
            <a href="/a/help" title="signin">捐助</a>
          </li>
      </div> 
  </div>
</template>

<script>
/* eslint-disable */
import URLprefix from '../../config'
export default {
  name: 'Mnav',
  data: function () {
    return { 
      signin_user: '',
      messages_count:''
    }
  },
  mounted: function() {
      let menu = document.getElementById('menu');
      menu.addEventListener('click', function() {
          let nav = document.getElementById('rnav');
              if (nav.style.height == 'auto') {
                  nav.style.height = '0';
              }else{
                  nav.style.height = 'auto';
              }
      }, false);

      if (localStorage.getItem('signin_user')){
          this.signin_user =  JSON.parse(localStorage.getItem('signin_user'))
          let user_id =  JSON.parse(localStorage.getItem('signin_user')).id
          let data = { user_id: Number.parseInt(user_id)}
          fetch(URLprefix + 'api/user/id/messages',{
                    body: JSON.stringify(data), 
                    headers: {
                      'content-type': 'application/json'
                    },
                    method: 'POST',
                    mode: 'cors'
                }).then(response => response.json())
                .then(json => {
                    this.messages_count = json.messages_count
                })
                .catch((e) => {
                  console.log(e)
                })
      }else{
        return
      }
  },
  methods: {
    logout() {
      if (localStorage.getItem('token')){
          localStorage.removeItem('token')
          localStorage.removeItem('signin_user')
          this.$router.push('/a/signin')
      }else{
        return
      }
    }
  }
}
</script>

<style lang="css" scoped>
a {
  color: inherit;
}
#line {
  position: fixed;
  width: 100%;
  height: 4px;
  background-color: green; 
}
#lnav {
    position: fixed;
    width: 100%;
    line-height: 60px;
    zoom:1;
    display: flex;
    background-color: #ffffff;
    justify-content: space-between;
    /* border-bottom: 1px solid var(--purple); */
    border-bottom: 1px solid #dfdcdc;  
    box-shadow: 0 3px 3px rgba(200, 216, 206, 0.12), 0 3px 3px rgba(97, 107, 100, 0.24);
  }
 #lnav #left #front img {
    width: 8rem;
    height: 3.3rem;
    vertical-align:middle;
  }
  /* #lnav #left #name {
    font-size: 2rem;
    margin: auto 1vw;
    font-weight: bold;
    vertical-align:middle;
  } */
  #lnav label {
    float: right;
    font-size: 26px;
    font-weight: bold;
    margin: auto 1vw;
  }
  #lnav label a {
    margin-right: 1vw;
  }
  #rnav li #mnumber { 
      font-size: 20px;
      background-color: fuchsia;
      color: gold;
      padding: 0 0.3rem;
  }
@media only screen and (max-width:  600px) {
    #lnav #dlnav #tail {
      font-size: 1.1rem;
      margin-left: 1.1rem ;
      font-weight: bold;
      vertical-align:middle;
    }
    #lnav #left #front img {
      margin-left: 1vw;
    }
    #lnav #mhome {
      padding-top: 0.2vh;
      font-size: 1.1rem;
      font-weight: bold;
    }
    #rnav li #mnumber { 
      padding: 1vh 4.4vw;
  }
    #rnav {
      padding-top: 3.8rem;
      height: 0;
      display: block;
      overflow: hidden;
    }
    #rnav li a, #rnav #mlnav a {
      display:block;
      font-size: 1.1rem;
      font-weight: bold;
      border-bottom: 1px solid green;
      padding: 1vh 4vw;
      color: var(--white);
    }
}
@media only screen and (min-width:  600px) and (max-width:  850px) {
    #menu, #mlnav,#mhome {
      display: none;
    }
    #lnav #left #front img {
      margin-left: 3vw;
    }
    #lnav #left #tail {
      font-size: 20px;
      margin: auto 1rem;
      font-weight: bold;
      vertical-align:middle;
    }
    #rnav {
      position: fixed;
      right: 0;
      line-height: 60px;
      padding-right: 3vw;
    }
    #rnav li {
      display: inline-block;
    }
    #rnav li a {
      font-size: 20px;
      font-weight: bold;
      padding-left: 2vw;
    }
}
@media only screen and (min-width:  850px) {
    #menu, #mlnav,#mhome {
      display: none;
    }
    #lnav #left #front img {
      margin-left: 10vw;
    }
    #lnav #left #tail {
      font-size: 20px;
      margin: auto 1rem;
      font-weight: bold;
      vertical-align:middle;
    }
    #rnav {
      position: fixed;
      right: 0;
      line-height: 60px;
      padding-right: 10vw;
    }
    #rnav li {
      display: inline-block;
    }
    #rnav li a {
      font-size: 20px;
      font-weight: bold;
      padding-left: 2vw;
      color: var(--grey);
    }
}
</style>
