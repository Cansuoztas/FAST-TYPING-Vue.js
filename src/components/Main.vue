<!-- eslint-disable vue/multi-word-component-names -->
<template>
 <div class="container jumbotron">
  <h1 class="display-4">Hızlı Yazma Yarışması</h1>
  <p class="lead">Ne kadar hızlı klavye kullandığını test et...</p>
  <hr class="my-4">
   <div v-if="isFinish" class="alert alert-primary">
    <h3>Süre Bitti</h3>
    <p class="display-4">{{dks}}</p>
    <span>Doğruluk Yüzdesi : % {{ truePercent }} </span>
    <span>Doğru Kelime : {{trueCount}} </span>
    <span>Yanlış Kelime : {{ falseCount }} </span>

    <button @click="newGame" class="btn btn-success btn-lg mt-5 btn-block"> YENİ OYUN </button>
   </div>
   <div v-else>
  <div  class="card">
    <div class="card-body">
        <span v-for="(word,key) in words.filter((data,index)=>index<20)" :key="key" v-bind:class="key!=0 || writingWordControl " class=" words m-2 " >{{word}}
        </span>
    </div>
  </div>
  <div class="card">
    <div class="card-body bg-secondary">
        <div class="input-group-lg">
      <input type="text" class="form-control" v-model="writingWord" >
  <div class="input-group-append" id="button-addon4">
    <button class="btn btn-light disabled" type="button">{{timer}}sn.</button>
    <button :disabled="isRunning" class="btn btn-light" type="button" @click="getWords">Yenile</button>
  </div>
</div>
    </div>
  </div>
</div>
</div>
</template>

<script>
import wordList from '@/assets/words.json'
export default {
data() {
  return {
    words:[],
    writingWord: null,
    isTrue: true,
    trueCount:0,
    falseCount: 0,
    timer : 60,
    interval:false,
    isRunning:false,
    isFinish: false,
    wordList:wordList
  }
},
watch:{
  writingWord(val){
    if(!val || val=== ' '){
        this.writingWord=''
        return
    }
    if(!this.isRunning) this.toggleTimer()
        const word = this.words[0].slice(0, val.length)
        const userWord = val.replace(' ','') //boşluğu sil
        //  const word = this.words[0].slice(0, val.length).toUpperCase()
        //  const userWord = val.replace(' ','') .toUpperCase() //büyük-küçük harf

          this.isTrue = word === userWord
        if(val.indexOf(' ') !== -1){
          this.isTrue ? this.trueCount++ : this.falseCount++ 
          this.words.splice(0,1)
          this.writingWord=''
          this.isTrue=true
        }
  }
},
computed:{
  writingWordControl(){
    return this.isTrue ? 'writing-word' : 'writing-word bg-danger'
  },
  dks (){
    return 300 - this.words.length
  },
  truePercent(){
    let percent =  (100/this.dks)
    const val =  (percent * this.trueCount)
    return isNaN(val) ? 0 : val
  }
},


mounted(){
  this.getWords()
},


methods:{

  newGame(){
    this.getWords()
    this.isFinish=false
    this.timer=60
    this.isTrue=true
    this.isRunning=false
  },

  getWords(){
this.words = this.wordList.sort(()=>Math.random()-0.5).splice(0,300)
  },
  toggleTimer(){
    this.isRunning=true
    this.interval= setInterval(this.timePrpcess,1000)
  },
  timePrpcess (){
    if(this.timer===0){
      clearInterval(this.interval)//sayacı eksiye düsürmeyecek
      this.isFinish=true
      return
    }
    this.timer--
  }
}
 }

</script>

<style>
    .words{
        font-size: 25px;
        font-weight: 400;
    }
    .writing-word{
      background-color: slategrey;
      color: white;
      padding: 5px;
      border-radius: 5px;

    }

</style>