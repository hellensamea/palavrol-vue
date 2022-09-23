<template>
    <div class="principal">
  <h2>PALAVROL</h2>
    <p>Neste jogo você tem que descobrir uma palavra aleatória. As dicas são: palavras com 5 letras, sem acentos ou ç. Boa sorte!</p>
    <div class="tentativa 1">
        <input type="text" class="upper" maxlength="5" id="1" required v-model="digitado" v-on:keypress.enter="verificarPalavra(digitado)" placeholder="digite uma palavra">
        
        <!--<input type="text" maxlength="1" id="2" required v-on:keyup="$event.target.nextElementSibling.focus()">
        <input type="text" maxlength="1" id="3" required v-on:keyup="$event.target.nextElementSibling.focus()">
        <input type="text" maxlength="1" id="4" required v-on:keyup="$event.target.nextElementSibling.focus()">
        <input type="text" maxlength="1" id="5" required>-->
    </div>
    <!--<div class="tentativa 2">
        <input type="text" maxlength="1" required v-on:keypress="proxLetra">
        <input type="text" maxlength="1">
        <input type="text" maxlength="1">
        <input type="text" maxlength="1">
        <input type="text" maxlength="1">   
    </div>
    <div class="tentativa 3">
        <input type="text">
        <input type="text">
        <input type="text">
        <input type="text">
        <input type="text">     
    </div>
    <div class="tentativa 4">
        <input type="text">
        <input type="text">
        <input type="text">
        <input type="text">
        <input type="text">    
    </div>
    <div class="tentativa 5">
        <input type="text">
        <input type="text">
        <input type="text">
        <input type="text">
        <input type="text">   
    </div>-->
    <div >
        <p v-show="resultado==='ok'">Você acertou!!!</p>
    </div>
    <div class="acertos" v-if="resultado">
            <p v-if="resultado=='nenhuma'">Letras corretas: <span>{{resultado}}</span></p>
            <p v-if="resultado!=='ok' && resultado!== 'nenhuma'">
                Você acertou: <span class="upper">{{resultado}}</span>
                <br> Letras que a palavra <span class="bold">não</span> contém: <span class="upper">{{erros.toString()}}</span>
            </p>
    </div>
    <div>
        <p>Palavras que você já acertou: <span v-if="lista.length>0">{{lista}}</span></p>
    </div>
</div>
</template>
<script>
    export default{
        name: "Home",
        data (){
            return {
                digitado: "",
                palavras: [],
                letrasCorretas: [],
                letrasDigitadas:[],
                erros:[],
                acertos: [],
                resultado:"",
                lista:[]
            }
        },
        methods: {
            async getPalavras(){
                const req = await fetch('http://localhost:3000/palavras/');
                const data = await req.json();
                this.palavras = data.nivel1;
            },

            verificarPalavra(digitado, letrasCorretas, letrasDigitadas, acertos,lista, erros){
                var correta = this.palavras[1].palavra
                if(digitado == correta){
                    this.acertou()
                    this.lista+=correta;
                }else{
                    letrasCorretas = correta.split(""),
                    letrasDigitadas = digitado.split(""),

                    acertos = letrasCorretas.filter(letra => letrasDigitadas.includes(letra))
                    
                    acertos.length>0?this.parcial(acertos):this.resultado='nenhuma',

                    this.erros+= this.letrasErradas(letrasCorretas, letrasDigitadas)+","
                }
            },
            acertou(){
                this.resultado='ok'
                setTimeout(() => this.resultado="", 3000)
                setTimeout(() => this.digitado="", 3000)
            },
            parcial(acertos){
                this.resultado += acertos.toString().replace(/,/g,", ")
            },
            letrasErradas(letrasCorretas, letrasDigitadas){
              return letrasDigitadas.filter(e => {
                return !letrasCorretas.includes(e);
                
              })
            }
              
        },
        mounted(){
            this.getPalavras();
        }
    }
</script>

<style>
.principal{
  background-color: rgba(255, 255, 255, 0.87);
  color: black;
  width: 500px;
  height:550px;
  border-radius: 10px;
  padding: 1em;
  flex-direction: column;
}
h2{
  background-color: lightgray;
  border-color: rgba(0, 0, 0, 0.459);
  width: 160px;
  margin: auto;
  padding: 0.3em;
  border-radius: 10px;
}
input{
  width: 180px;
  height: 35px;
  color: rgba(0, 0, 0, 0.808);
  font-weight: bolder;
  font-size: 1em;
  text-align: center;
  justify-content: center;
  background-color: lightgray;
  border-color: rgba(0, 0, 0, 0.459);
  border-width: 0.5px;
  display: flex;
  margin: 0.5px;
  border-radius: 10px;
}
.tentativa{
  flex-direction: row;
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}
.acetos{
    justify-content: center;
    display: flex;
    align-content: center;
    align-items: center;
    flex-direction: column;
}
.upper{
    text-transform: uppercase;
}
.bold{
    font-weight: bold;
}
</style>