<template>
<div class="addWords">
<form>
    <label>Слово
    <input type="text" id="word" v-on:keypress.enter="addWord" placeholder="Enter a english word"  v-model="newback"/>
    </label>
    <label>Перевод
    <input type="text" id="translation" v-on:keypress.enter="addWord" placeholder="Enter a translation" v-model="newfront"/>
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
    <li :class="{line: isDone(word)}" v-for="(word, index) in words" :key='index' @click="active(word)">
          <img src="../assets/remove.png" class="remove" alt="" v-on:click="remove(index)">
          <input @click="word.done= !word.done" v-model="word.done" type="checkbox" id="checkbox" name="done" />
          <transition name="flip">
          <p v-bind:key="word.active" class="word">{{word.active ? word.front : word.back}}
          </p>
          </transition>
    </li>
</ul>
</div>
</template>
<script>
export default {
  name: 'Distionary',
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
          done: false
        },
        {
          back: 'home',
          active: false,
          front: 'дом',
          done: false
        },
        {
          back: 'orange',
          active: false,
          front: 'апельсин',
          done: false
        },
        {
          back: 'mother',
          active: false,
          front: 'мама',
          done: false
        },
        {
          back: 'father',
          active: false,
          front: 'папа',
          done: false
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
          done: false})
        this.error = false
        this.error2 = false
        this.done = false
      }
      this.newback = this.newfront = ''
      this.saveWords()
    },
    active (word) {
      word.active = !word.active
      this.saveWords()
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
#checkbox {
  cursor: pointer;
  width: 20px;
  height: 20px;
  float: right;
  padding: 5px;

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
}
li img{
  cursor: pointer;
  width: 20px;
  height: 20px;
  float: right;
  padding: 5px}

li p {
  overflow: hidden;
text-overflow: ellipsis;
padding-top: 20px;
cursor: pointer;
}
li{
    min-width: 200px;
    height: 200px;
    background: rgb(0, 192, 80);
    margin: 5px;
    list-style-type: none;
    border: 1px solid black;
    border-radius: 10px;
    font-size: 40px;
    overflow: hidden;
    color: #ffffff;
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
  text-decoration: line-through;
  color:#8d918e
}
.line:hover{
  background:#00FF00;
  text-decoration: line-through;
}
</style>
