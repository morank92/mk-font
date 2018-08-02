<template>
  <div>
    <Row>
      <Col span="24">
      <Card :bordered="false" style="max-height:600px;min-height:500px;">
            <p slot="title">WeChat card</p>
            <p v-for="(m,index) in r_msg" >{{m}}</p>
        </Card>
        </Col>
    </Row>
    <Row style="margin-top:50px;">
      <Input v-model="s_msg" placeholder="Enter something..." style="width: 600px;" /> <Button type="info" @click="sendMsg()">发送</Button>
    </Row>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      r_msg:[],
      s_msg:"",
      msg: 'Welcome to Your Vue.js App',
      ws:null,
    }
  },
  mounted:function(){
    this.initWebsocket();
  },
  methods:{
    initWebsocket () {
          let ws = new WebSocket('ws://localhost:9001/websocket');
          this.ws = ws;
          let that = this;
          ws.onopen = () => {
              // Web Socket 已连接上，使用 send() 方法发送数据
                console.log('数据发送中...')
            }
            ws.onmessage = evt => {
              console.log(evt);
              that.r_msg.push(evt.data);
              console.log('数据已接收...')
            }
            ws.onclose = function () {
              // 关闭 websocket
              console.log('连接已关闭...')
            }
            // 组件销毁时调用，中断websocket链接
            this.over = () => {
              ws.close()
            }
      },
      sendMsg(){
        if(this.s_msg != ""){
          this.ws.send(this.s_msg);
        }
        
        this.s_msg = "";
      },
      wsOpen(){
      },
      wsMessage(){

      },
      wsClose(){

      },

      
  },
  beforeDestroy () {
    this.over()
}



}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
</style>
