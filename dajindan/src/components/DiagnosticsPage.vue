<template>

  <div>
    <div class="title">
      <div class="questionsNumber">
        <span>{{`Q. ${currentQNumber + 1} of ${maxNumber}`}}</span>
      </div>
      <div>
        <span>{{diagnosticName}}</span>
      </div>
      <button @click="toMainMenu">다른 검사하기</button>
    </div>

    <div class="question">
      <span>지난 2주 동안에<br /><br />{{getQuestions()}}</span>
    </div>

    <div class="contour"></div>

    <div class="questionWrap">
      <div class="answerList" :class="{ answerClick: isClickItem === i }" v-for="선택옵션, i in getOptions()" :key="선택옵션">
        <button @click="answerItem(i)">{{선택옵션}}</button>
      </div>
    </div>

    <div class="arrowLeft">
      <button class="arrowBtn" @click="previousQuestions">
        <svg width="80%" height="100%" aria-hidden="true" fill="white" viewBox="0 0 20 20"
          xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd"
            d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
            clip-rule="evenodd"></path>
        </svg>
      </button>
    </div>

    <div class="arrowRight">
      <button class="arrowBtn" @click="nextQuestions">
        <svg width="80%" height="100%" aria-hidden="true" fill="white" viewBox="0 0 20 20"
          xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd"
            d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
            clip-rule="evenodd"></path>
        </svg>
      </button>
    </div>
  </div>

</template>

<script>

import DData from '../assets/DData.json'

export default {
  name: 'DiagnosticsPage',
  data() {
    return {
      jindanData: DData,
      currentQNumber: 0,
      maxNumber: 9,
      isClickItem: 99,
      diagnosticID: this.getdiagnosticID(),
    }
  },
  props: {
    diagnosticName: String,
  },
  methods: {
    nextQuestions() {
      if (this.currentQNumber + 1 >= this.jindanData.PHQ.length) {
        alert("결과를 확인하시겠습니까?")
        return
      }
      this.currentQNumber++
    },
    previousQuestions() {
      if (this.currentQNumber - 1 < 0) {
        alert("첫번째 항목입니다!")
        return
      }
      this.currentQNumber--
    },
    toMainMenu() {
      this.$emit("setDiagnostic", "main")
    },
    answerItem(index) {
      if(this.isClickItem === index) {
        this.isClickItem = 99
        return
      }
      this.isClickItem = index
    },
    getdiagnosticID() {
      if(this.diagnosticName === 'PHQ-9(우울증)') return 'PHQ'
      else return 'unknown'
    },
    getQuestions() {
      if(this.diagnosticID === 'unknown') return "데이터 불러오기 실패"
      else return this.jindanData[this.diagnosticID][this.currentQNumber].questions
    },
    getOptions() {
      if(this.diagnosticID === 'unknown') return "데이터 불러오기 실패"
      else return this.jindanData[this.diagnosticID][this.currentQNumber].options
    }
  },
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  border-radius: 10px;
  background-color: #2c3e50;
  padding: 1rem;
  color: white;
  text-align: center;
  font-size: xx-large;
  font-weight: bold;
  margin-bottom: 1rem;
}

.questionsNumber {
  background-color: #E9967A;
  border-radius: 10px;
  padding: 0.3rem 0.5rem 0.3rem 0.5rem;
  font-size: 1.9rem;
  line-height: 1;
}

.question {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 12rem;
  background-color: #57687c;
  border-radius: 10px;
  padding: 1rem;
  font-size: xx-large;
  font-weight: bold;
  text-align: center;
  color: white;
}

.contour {
  border: 1px dashed black;
  margin: 1rem 0 1rem 0;
}

.questionWrap {
  display: grid;
  flex-grow: 1;
  gap: 1rem;
}

.answerList button {
  width: 100%;
  height: 100%;
  padding: 1rem;
  border: none;
  border-radius: 10px;
  background-color: #E1E2E1;
  font-size: x-large;
  font-weight: bold;
  cursor: pointer;
}

.answerList button:hover {
  background-color: #afb0af;
}

.answerClick button {
  background-color: #E9967A !important;
  color: white;
}

.arrowLeft {
  display: flex;
  position: fixed;
  justify-content: center;
  align-items: center;
  width: 15%;
  height: 100%;
  top: 0;
  left: 0;
}

.arrowRight {
  display: flex;
  position: fixed;
  justify-content: center;
  align-items: center;
  width: 15%;
  height: 100%;
  top: 0;
  right: 0;
}

.arrowBtn {
  padding: 0;
  margin: 0;
  border: none;
  background-color: transparent;
  cursor: pointer;
}

.arrowBtn:hover svg {
  fill: #E9967A;
}
</style>