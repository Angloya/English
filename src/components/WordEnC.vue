<template>
<div class="addWords">
<form>
    <label>Слово
    <input type="text" id="word" v-on:keypress.enter="addWord" placeholder="Enter a english word"  v-model="newback"/>
    </label>
    <label>Перевод
    <input type="text" id="word" v-on:keypress.enter="addWord" placeholder="Enter a translation" v-model="newfront"/>
    </label>
    <img src="../assets/Plus.png" class="add" @click="addWord">
</form>
<p v-show="error" class="error">Нужно заполнить оба поля</p>
<p v-show="error2" class="error">Такое слово уже есть</p>
<span class="removeAll">
<p> Удалить все слова</p>
<img src="../assets/remove.png"  alt="" v-on:click="removeAll(index)">
 </span>
 <span class="mark-all"> Все слова были изучены<input type="checkbox" id="mark-all" @click="selectAll">
</span>
<ul>
    <li :class="{line: isDone(word)}" v-for="(word, index) in words" :key='index'>
      <div class="remove">
          <img src="../assets/remove.png" class="remove" alt="" v-on:click="remove(index)">
           <img src="../assets/help.png" class="remove" alt="" @click="active(word)">
           </div>
          <div class="card">
            <transition name="flip">
            <p v-bind:key="word.active" class="word">{{word.active ? word.back : word.front}}</p>
            </transition>
            <div class="input">
            <input class="translation" type="text" required id="translation" v-on:keypress.enter="checked(word)" placeholder="Enter a word" v-model="word.checkback" />
            <img v-show="!word.done" src="../assets/Plus.png" class="add" @click="checked(word)">
            </div>
           <span v-show="word.right2" class="right">Неверно</span>
           <span v-show="word.right" class="right">Верно! Молодец!</span>
          </div>
    </li>
</ul>
</div>
</template>
<script>
export default {
  name: 'WordEn',
  data () {
    return {
      newback: '',
      newfront: '',
      error: false,
      error2: false,
      words: [
        {
          back: 'apple',
          active: false,
          front: 'яблоко',
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'home',
          active: false,
          front: 'дом',
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'orange',
          active: false,
          front: 'апельсин',
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'mother',
          active: false,
          front: 'мама',
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'father',
          active: false,
          front: 'папа',
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'car',
          front: 'машина',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'hope',
          front: 'надежда',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'how is it going?',
          front: 'как идут дела?',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'talk to you later!',
          front: 'поговорим с тобой позже!',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'all the same',
          front: 'без разницы',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'believe it or not, but',
          front: 'верите или нет, но',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'in a way / to a certain extent',
          front: 'в каком-то смысле',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'it’s a great idea!',
          front: 'отличная идея!',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'thank you in advance!',
          front: 'спасибо заранее!',
          active: false,
          done: false,
          right: false,
          right2: false
        },
        {
          back: 'after you!',
          front: 'после вас!',
          active: false,
          done: false,
          right: false,
          right2: false
        }
      ]
    }
  },
  mounted () {
    if (localStorage.getItem('words')) {
      try {
        this.words = JSON.parse(localStorage.getItem('words'))
      } catch (e) {
        localStorage.removeItem('words')
      }
    }
  },
  methods: {
    checkedWord (word, side) {
      for (var item of this.words) {
        if (item[side] === word) {
          return true
        }
      }
      return false
    },
    checkedWord2 (id, check, side) {
      if (id[side] === check) {
        return true
      }
      return false
    },
    checked (word) {
      if (this.checkedWord2(word, word.checkback, 'back')) {
        word.right = true
        word.right2 = false
        word.done = true
        this.saveWords()
      } else {
        word.right2 = true
        word.right = false
        word.done = false
        word.checkback = ''
      }
    },
    addWord () {
      if (!this.newfront || !this.newback) {
        this.error = true
      } else if (this.checkedWord(this.newfront, 'front') || this.checkedWord(this.newback, 'back')) {
        this.error2 = true
      } else {
        this.words.push({
          back: this.newback,
          front: this.newfront,
          active: false,
          done: false,
          right: false,
          right2: false
        })
        this.error = false
        this.error2 = false
        this.done = false
      }
      this.newback = this.newfront = ''
      this.saveWords()
    },
    active (word) {
      word.active = !word.active
    },
    remove (id) {
      this.words.splice(id, 1)
      this.saveWords()
    },
    removeAll (id) {
      this.words.splice(id)
      this.saveWords()
    },
    saveWords () {
      const parsed = JSON.stringify(this.words)
      localStorage.setItem('words', parsed)
    },
    isDone: function (word) {
      return word.done
    },
    selectAll: function (word) {
      this.done = !this.done
      for (var i = 0; i < this.words.length; i++) {
        this.words[i].done = this.done
      }
      this.saveWords()
    }
  }
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300,300i');
.addWords{
font-family: 'Open Sans Condensed', sans-serif;
}
.mark-all {
 display: flex;
 justify-content: center;
 align-items: center;
  }
#mark-all{
  margin-top: 5px;
  cursor: pointer;
  width: 20px;
  height: 20px;
}
.add {
    padding-left:10px;
    width: 60px;
}
.removeAll{
   display: flex;
  align-items: center;
  justify-content: center;
}
.removeAll img{
  width: 30px;
  height: 30px;
  cursor: pointer;
}
form{
  display: flex;
  align-items:stretch;
  justify-content: center;
}
.flip-enter-active{
transition: all 0.6s ease;
}
.flip-enter{
  transform: rotateY(180deg);
  opacity: 0;
}
.flip-leave{
display: none;
}
ul {
    display: flex;
    justify-content: center;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    color: black;
}
h1{
  font-size: 40px;
  font-weight: 300;
  color: black;
}
input{
    width: 200px;
    height: 40px;
    font-weight: 300;
    font-size: 15px;
    text-transform: lowercase;
}
li img{
  cursor: pointer;
  width: 20px;
  height: 20px;
  padding: 5px}

li p {
  overflow: hidden;
text-overflow: ellipsis;
cursor: pointer;
margin: 0px;
padding: 0px;
padding-bottom: 10px;
}
.card div{
 display: flex;
 flex-direction: column;
 justify-content: center;
 align-content: center;
 align-items: center;
}
.card .add{
  width: 40px;
  height: 40px;
  margin-bottom: 0px;
  padding-bottom: 0px;
}
.remove{
  float: right;
}
li{
    min-width: 300px;
    min-height: 200px;
    background: rgb(0, 192, 80);
    margin: 5px;
    list-style-type: none;
    border: 1px solid black;
    border-radius: 10px;
    font-size: 40px;
    overflow: hidden;
    color: #ffffff;
    display: flex;
    flex-direction: column;
    padding: 10px;
    text-transform: lowercase;
}
li:nth-child(2n+2) {
background: #20B2AA;
}
li:nth-child(5n-2) {
background:#7B68EE;
}
li:nth-child(4n+1) {
background:#00BFFF;
}
li:nth-child(5n+4) {
background:#00FA9A;
}
li:hover{
 background:#7FFFD4;
-webkit-transform: scale(1.3);
  -ms-transform: scale(1.3);
  transform: scale(1.3);
  font-size: 40px;
  transition:0.4s;
transform-style: preserve-3d;
color: #000000;
}
label{
    padding: 10px;
    font-size: 20px;
}
.line{
  background:#00FF00 !important;
  color:#8d918e
}
.line:hover{
  background:#00FF00;
}
div .input {
  display: flex;
  flex-direction: row;
  align-items: center;
   padding-left: 20px;
}
div .input img {
  padding-left: 10px;
}
#translation {
background: rgba(202, 255, 252, 0.3);
border: 1px solid rgba(125, 177, 159, 0.5);
border-radius: 10px;
padding:5px;
text-transform: lowercase;
}
</style>
