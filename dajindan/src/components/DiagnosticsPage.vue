<template>

  <div>
    <div class="title">
      <div class="questionsNumber" v-if="isResult === false">
        <span>{{`Q. ${currentQNumber + 1} of ${maxNumber}`}}</span>
      </div>
      <div class="questionsNumber" v-else>
        <span>{{`진단 결과`}}</span>
      </div>
      <div>
        <span>{{diagnosticName}}</span>
      </div>
      <button @click="toMainMenu">다른 검사하기</button>
    </div>

    <div class="question">
      <div v-if="isResult === false">
        <span>지난 2주 동안에<br /><br />{{getQuestions()}}</span>
      </div>
      <div v-else>
        <span>{{`나의 점수: ${resultPoints}`}}<br /><br />{{getSeverity()}}</span>
      </div>
    </div>

    <div class="contour"></div>

    <div v-if="isResult === false" class="questionWrap" :class="{ bordering: isNoChoose }">
      <div class="answerList" :class="{ answerClick: isClickItem === i }" v-for="선택옵션, i in getOptions()" :key="선택옵션">
        <button @click="answerItem(i)">{{선택옵션}}</button>
      </div>
      <div v-if="isNoChoose" class="noChoose">
        <span>문항을 선택해 주세요</span>
      </div>
    </div>
    <div v-else class="questionWrap">

      <div class="judgmentWrap">
        <div class="headerItem"><span>PHQ-9 점수</span></div>
        <div class="headerItem"><span>심각도 평가</span></div>
        <div class="desItem"><span>0 ~ 4</span></div>
        <div class="desItem"><span>우울증 아님</span></div>
        <div class="desItem"><span>5 ~ 9</span></div>
        <div class="desItem"><span>가벼운 우울증</span></div>
        <div class="desItem"><span>10 ~ 19</span></div>
        <div class="desItem"><span>중간정도 우울증</span></div>
        <div class="desItem"><span>20 ~27</span></div>
        <div class="desItem"><span>심한 우울증</span></div>
      </div>
    </div>

    <div class="arrowLeft" v-if="isResult === false">
      <button class="arrowBtn" @click="previousQuestions">
        <svg width="80%" height="100%" aria-hidden="true" fill="white" viewBox="0 0 20 20"
          xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd"
            d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
            clip-rule="evenodd"></path>
        </svg>
      </button>
    </div>

    <div class="arrowRight" v-if="isResult === false">
      <button class="arrowBtn" @click="nextQuestions">
        <svg width="80%" height="100%" aria-hidden="true" fill="white" viewBox="0 0 20 20"
          xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd"
            d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
            clip-rule="evenodd"></path>
        </svg>
      </button>
    </div>

    <CustomModal v-if="modalVis" @toWhere="toMainEmit" @setModalHidden="setModalHidden" :moodalOption="moodalOption"
      @setResult="setResult">
    </CustomModal>
  </div>

</template>

<script>

import DData from '../assets/DData.json'
import CustomModal from './CustomModal.vue'

export default {
  name: "DiagnosticsPage",
  data() {
    return {
      jindanData: DData,
      currentQNumber: 0,
      maxNumber: 9,
      diagnosticID: this.getdiagnosticID(),
      modalVis: false,
      moodalOption: {
        msg: '',
        selection: [
          { buttonMsg: "네", buttonAction: true },
          { buttonMsg: "아니오", buttonAction: false }
        ]
      },
      isNoChoose: false,
      answerSheet: [],
      isClickItem: 99,
      isResult: false,
      resultPoints: 0
    };
  },
  props: {
    diagnosticName: String,
  },
  methods: {
    getPoints() {
      let sum = 0
      for(let i=0; i < this.answerSheet.length; i++) {
        sum += this.answerSheet[i].answer
      }
      return sum
    },
    getSeverity() {
      if(this.resultPoints < 5) return "우울증 아님"
      else if(this.resultPoints < 10) return "가벼운 우울증"
      else if(this.resultPoints < 20) return "중간정도 우울증"
      else return "심한 우울증"
    },
    setResult(boolean) {
      this.isResult = boolean
    },
    nextQuestions() {
      if (this.isClickItem === 99) {
        this.isNoChoose = true
        return
      }

      this.answerSheet[this.currentQNumber] = { qNum: this.currentQNumber, answer: this.isClickItem }

      if (this.currentQNumber + 1 >= this.jindanData.PHQ.length) {
        this.moodalOption = {
          msg: '결과를 확인하시겠습니까?',
          selection: [
            { buttonMsg: "네", buttonAction: true },
            { buttonMsg: "아니오", buttonAction: false }
          ]
        }
        this.resultPoints = this.getPoints()
        this.modalVis = true
        return
      }

      this.currentQNumber++;
      this.isClickItem = this.answerSheet[this.currentQNumber]?.answer ?? 99
    },
    previousQuestions() {
      if (this.currentQNumber - 1 < 0) {
        this.moodalOption = {
          msg: '첫 항목입니다!',
          selection: [
            { buttonMsg: "확인하기", buttonAction: false }
          ]
        }
        this.modalVis = true
        return
      }
      this.currentQNumber--;
      this.isClickItem = this.answerSheet[this.currentQNumber]?.answer ?? 99
    },
    toMainMenu() {
      this.moodalOption = {
        msg: '진행중인 진단이 있습니다.\n정말로 나가시겠습니까?',
        selection: [
          { buttonMsg: "네", buttonAction: true },
          { buttonMsg: "아니오", buttonAction: false }
        ]
      }
      this.modalVis = true
    },
    answerItem(index) {
      console.log(this.answerSheet)
      if (this.isClickItem === index) {
        this.isClickItem = 99;
        return;
      }
      this.isNoChoose = false
      this.isClickItem = index;
    },
    getdiagnosticID() {
      if (this.diagnosticName === "PHQ-9(우울증)")
        return "PHQ";
      else
        return "unknown";
    },
    getQuestions() {
      if (this.diagnosticID === "unknown")
        return "데이터 불러오기 실패";
      else
        return this.jindanData[this.diagnosticID][this.currentQNumber].questions;
    },
    getOptions() {
      if (this.diagnosticID === "unknown")
        return "데이터 불러오기 실패";
      else
        return this.jindanData[this.diagnosticID][this.currentQNumber].options;
    },
    setModalHidden(aciton) {
      this.modalVis = aciton
    },
    toMainEmit() {
      this.$emit("setDiagnostic", 'main')
    }
  },
  components: {
    CustomModal
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hidden {
  display: none;
}

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
  position: relative;
  display: grid;
  flex-grow: 1;
  gap: 1rem;
}

.noChoose {
  position: absolute;
  top: 0;
  left: 0;
  background-color: #E9967A;
  border-radius: 5px;
  color: white;
  padding: 0.2rem;
}

.bordering {
  border: 2px solid #E9967A;
  border-radius: 10px;
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

.resultWrap {
  position: relative;
}

.judgmentWrap {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  border-top: 2px solid #2c3e50;
  border-left: 2px solid #2c3e50;
  text-align: center;
}

.judgmentWrap>div {
  border-right: 2px solid #2c3e50;
  border-bottom: 2px solid #2c3e50;
}

.headerItem {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  font-size: x-large;
  font-weight: bold;
  background-color: #afb0af;
}

.desItem {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  font-size: x-large;
}
</style>