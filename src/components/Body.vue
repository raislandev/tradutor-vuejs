<template>
  <div class="body">
    <div class="container">
       <div id="progress" >
         
       </div>
       <div v-for="live in lives" :key="live" class="img">
         <img v-if="live" src="../assets/coracao_cheio.png" alt="">
         <img v-if="!live" src="../assets/coracao_vazio.png" alt="">
       </div>      
       <div class="form">
          <p id="title">{{ title }}:</p>
          <p v-if="live_qtd < 3">{{ frases[count].fraseEng }}</p>
          <p v-if="live_qtd == 3">Suas chances acabaram, tente novamente!</p>
          <textarea name="frase" id="frase" cols="108" rows="5" v-model="frase"></textarea>
       </div>
       <div>
          <button v-if="perce<100 && live_qtd < 3 " @click="addCount">Verificar</button>
          <button v-if="perce==100 || live_qtd == 3" @click="reboot">Reiniciar</button>
       </div>
    </div>
  </div>
</template>

<script>
export default {
 
  data:function(){
        return{
            title:"Traduza esta frase",
            frases:[
              {fraseEng:'I like to learn', frasePt: 'Eu gosto de aprender'},
              {fraseEng:'I watch tv', frasePt: 'Eu assisto tv'},
              {fraseEng:'How are you?', frasePt: 'Como vai você'},
              {fraseEng:'I eat breand', frasePt: 'Eu como pão'},
              {fraseEng:'Parabéns você conclui o teste com sucesso!', frasePt: 'terminou'},

            ],
            perce:0,
            count:0,
            frase:"",
            lives:[true,true,true],
            live_qtd:0
            
        }
    },

    /*computed:{
       getPerc:function(){

       }
    },*/
    methods:{
       addCount(){
         if(this.frases[this.count].frasePt.toLowerCase() == this.frase.toLowerCase()){
           this.count = this.count+1
           this.perce = this.perce+25
           var porcentagem = this.perce+"%"
           document.documentElement.style.setProperty('--progress-bar', porcentagem);
           this.frase = ""
         }else{
           alert("Tradução errada, Tente Novamente!")
           this.live_qtd= this.live_qtd+1
           this.frase = ""
           for(var i=this.lives.length;i>=0;i--){
             if(this.lives[i]){
               this.lives.splice(i, 1, false)
               break
             }
           }
         }
         
       },
       reboot(){
         this.perce=0
         this.frase=""
         this.count=0
         document.documentElement.style.setProperty('--progress-bar', this.perce);
         this.lives = [true,true,true]
         this.live_qtd= 0
         
       } 

    }

}
</script>

<style>

:root{
    --progress-bar: 0%;
}


.body{
    margin: 0 auto;
    background-color: rgb(230, 230, 230);
    width: 75%;
    height: 320px;
    display: flex;
}
.container{
  padding: 35px;
}

#title{
  font-weight: bold;
}
button{
  display: flex;
  float: right;
  background-color: rgb(25, 153, 175);
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}

#progress{
    margin: 0 auto;
    background-color: rgb(235, 248, 215);
    border: 1px solid rgb(69, 221, 69);
    height: 20px;
    width: 50%;
    border-radius: 5px;
    float: left;
    display: flex;
    
}
#progress::before{
  content: "";
  /* Por enquanto colocamos
    uma largura e uma cor
    com valores fixos
    */
  width:var(--progress-bar);
  background-color: rgb(26, 153, 10);
  border-radius: 5px;

}

.form{
  float: left;
}

.img{
  float: right;
}

#frase{
  width: 100%;
}

</style>