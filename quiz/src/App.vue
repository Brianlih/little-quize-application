<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col>
          <!--"v-if"用來確保questions已經吃到API傳來的data-->
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import QuestionBox from "./components/QuestionBox";
import Header from "./components/Header";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0, //for 題號，我們只希望QuestionBox吃到目前題目即可，並render出來
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      //equals to "next: function(){}"
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "get",
    })
      .then((res) => {
        //為了讓內容顯示於console裡
        return res.json();
      })
      //上下兩者順序必須固定，否則無法顯示
      .then((jsonData) => {
        //真正去取值到questions array裡
        this.questions = jsonData.results;
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
