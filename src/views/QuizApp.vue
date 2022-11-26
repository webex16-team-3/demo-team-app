<template>
  <h1>Vue クイズ</h1>
  <div class="app">
    <h2>Q. {{ quiz[quizNumber].text }}</h2>
    <img
      class="quiz-image"
      v-bind:src="quizImagePath"
      v-bind:alt="quiz[quizNumber].text"
    />
    <div class="container">
      <button
        v-for="(choice, i) in quiz[quizNumber].choices"
        v-bind:key="i"
        v-on:click="choiced(choice)"
      >
        {{ choice.text }}
      </button>
    </div>
    <div>{{ feedback }}</div>
    <button v-if="!isHidden" v-on:click="showNextQuiz(quizNumber)">
      次の問題へ
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //「次の問題へ」ボタンは普段隠されている
      isHidden: true,
      feedback: "",
      //今の問題数を表示する変数を用意する
      quizNumber: 0,
      quiz: [
        {
          text: "この星の名前は何でしょう？",
          image: "Ganymede.jpg",
          choices: [
            {
              text: "ゴリアテ",
              isCorrect: false,
              feedback:
                "残念！ゴリアテは、旧約聖書に登場するダビデに石で殺される巨人だよ。",
            },
            {
              text: "ゼニガメ",
              isCorrect: false,
              feedback:
                "残念！ゼニガメは、クサガメまたはニホンイシガメの幼体だよ。",
            },
            {
              text: "ガニメデ",
              isCorrect: true,
              feedback: "正解！ガニメデは、木製の第三惑星だよ！",
            },
          ],
        },
        {
          text: "いま、何問目？",
          image: "Two.jpeg",
          choices: [
            {
              text: "１",
              isCorrect: false,
              feedback: "残念！ひとつ少ないよ。",
            },
            {
              text: "２",
              isCorrect: true,
              feedback: "正解！１でも３でもないよ！",
            },
            {
              text: "３",
              isCorrect: false,
              feedback: "残念！ひとつ多いよ。",
            },
          ],
        },
        {
          text: "この城の名前は？",
          image: "Maruoka.png",
          choices: [
            {
              text: "丸岡城",
              isCorrect: true,
              feedback: "正解！どこからどうみても丸岡城だね。",
            },
            {
              text: "丸亀城",
              isCorrect: true,
              feedback: "残念！どこからどうみても丸亀城ではないよ。",
            },
            {
              text: "丸子城",
              isCorrect: false,
              feedback: "残念！どこからどうみても丸子城ではないよ。",
            },
          ],
        },
      ],
    }
  },
  methods: {
    choiced(choice) {
      this.feedback = choice.feedback
      //その問題が正解かどうかを判定する
      if (choice.isCorrect) {
        if (this.quizNumber < this.quiz.length - 1) {
          //正解なら次の問題を出題するボタンが表示される
          this.isHidden = false
        }
      } else {
        this.isHidden = true
      }
    },
    //正解したら次の問題が出るようにしたい
    //ボタンを押下すると次の出題に切り替わる。
    showNextQuiz(quizNumber) {
      if (quizNumber < this.quiz.length - 1) {
        this.isHidden = true
        this.quizNumber = this.quizNumber + 1
        console.log(quizNumber)
      } else {
        this.isHidden = true
      }
    },
  },
  computed: {
    quizImagePath() {
      return require("./images/" + this.quiz[this.quizNumber].image)
    },
  },
}
</script>

<style>
.app {
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.quiz-image {
  height: 300px;
  width: 300px;
  object-fit: contain;
}
.container {
  display: flex;
  height: 2em;
  width: 300px;
  padding: 1em;
  justify-content: space-around;
}
</style>
