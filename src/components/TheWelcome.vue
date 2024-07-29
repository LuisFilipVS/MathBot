<script>
export default {
    data: () => ({
        messageVal : "",
        resposta : "",
        resposta2: "", 

        msgs: [
            // {
            //     type: "u",
            //     msg: "Hello World"

            // },
            // {
            //     type: "f",
            //     msg:  "Boa noite"
            // }
        ]
    }),
    methods: {
        sendMsg: function (input){

        // axios
        // .get(`http://localhost:5000/MathBot/response/${this.messageVal}`)
        // .then(response => (response.json()))
        // .then(data => (console.log(data)))

        fetch(`http://localhost:5000/MathBot/response/${this.messageVal}`)
          // .then(function(response) {
          //     if (!response.ok) {
          //     throw new Error(response.statusText);
          //     }

          //     return response.json();
          // })
          .then((response) => response.json())
          .then((data) => (
                console.log(`>>> ${data.response}`),
                this.refreshChat(input,data.response)
            ))
            .catch((error) => {
            console.error("DeuErro -> " + error);
            this.refreshChat(input,error)

            })


        this.updateScroll()
        this.messageVal = ""
        
        },
        refreshChat: function(input,response){
            this.msgs.push({
            type: 'u',
            msg: input
            })

            this.msgs.push({
            type: 'f',
            msg: response
            })
        },
        updateScroll: function(){
            var element = document.getElementById("app")
            window.scrollTo(0, document.body.scrollHeight);

        }
      }
}
</script>

<template>
      <div id="app">
        <div class="header">
            <div class="logo">
                <img  src="/logo_ifba.png" alt="">
            </div>
            <h2 class="title is-2">Mathbot Chat</h2>
            <div style="margin-right: 10px">By Luis Filipe <br> Profº : Luis Paulo</div>
        </div>
        <div class="container" id="boxChat">
            <div class="messages" id="test">
                <div v-for="m in msgs">                
                    <div class="chat" :class="{'chat-u' : m.type == 'u', 'chat-f': m.type=='f'}">{{m.msg}}</div>
                    <div class="clr"></div>
                </div>
            </div>
        </div> 
        <div class="text-part">
            <div class="columns" >
                <div class="column is-12">
                    <input type="text" v-model="messageVal" class="input" @keyup.enter="sendMsg(messageVal)" id="chatinput" placeholder="Digite Alguma coisa">
                </div>
                <div class="column is-3">
                    <button class="button is-success is-fullwidth" @click="sendMsg(messageVal)">Enviar</button>
                </div>
            </div>

        </div>

    </div>
</template>

<style scoped>
.container {
    height: 80%;
    max-width: 400px;
    margin: 50px auto;
    padding: 0 10px 0 10px 0!important;
    overflow-x:hidden;
    overflow-y: auto;
    /* margin-bottom: 200px; */
    padding-top: 100px;
    padding-bottom: 200px;
    z-index: 0;
}

.logo{
    width: 80px;
    padding-left: 20px;
}

.header{
    position:fixed;
    /* background-color: #fff; */
    top: 0%;
    padding-bottom: 20px;
    /* background :rgb(238, 238, 238); */
    left :0;
    padding-top: 20px;
    width:100%;
    z-index: 10;
    background-color: #14161a;
    display: flex;
    justify-content: space-between;
}

.chat {
    position: relative;
    border-radius: .4em;
    padding: 10px;
    margin-top: 20px;
    z-index: 10;
}
.chat-u {
    background: #00aabb;
    color: #fff;
    max-width: 80%;
    float: right;
    /* width: 50%; */
    text-align: right;
    z-index: 0;
}
.chat-f {
    background: #dddddd;
    color: black;
    max-width: 80%;
    float: left;
}
.chat-u:after {
    content: '';
	position: absolute;
	right: 0;
	top: 50%;
	width: 0;
	height: 0;
	border: 4px solid transparent;
	border-left-color: #00aabb;
	border-right: 0;
	margin-top: -4px;
	margin-right: -4px;
}

.chat-f:after {
    content: '';
	position: absolute;
	left: 0;
	top: 50%;
	width: 0;
	height: 0;
	border: 4px solid transparent;
	border-right-color: #dddddd;
	border-left: 0;
	margin-top: -4px;
	margin-left: -4px;
}
.clr{clear:both;}

.text-part {
    position: fixed;
    bottom: 0;
    padding-bottom: 1px;
    background :rgb(238, 238, 238);
    left :0;
    margin-top: 10px;
    width:100%;

}
.text-part .columns {

    min-width: 80%;
    float: left;
    margin: 0 auto;
}

.text-part .column {
    float: right;
    margin: 0 auto;
}
</style>
