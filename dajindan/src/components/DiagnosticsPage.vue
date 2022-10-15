<template>

  <div class="title">
    <button>메인메뉴</button>
    <div>
      <span>{{diagnosticName}}</span>
    </div>
    <div class="questionsNumber">
      <span>1/20</span>
    </div>
  </div>

  <div class="question">
    <span>지난 2주 동안에<br/>기분이 가라앉거나, 우울하거나, 희망이 없다고 느꼈다.</span>
  </div>

  <div class="contour"></div>

  <div class="questionWrap">
    <div class="answerList" v-for="선택옵션 in option" :key="선택옵션">
      <button onclick="">{{선택옵션}}</button>
    </div>
  </div>

</template>

<script>

export default {
  name: 'DiagnosticsPage',
  data() {
    return {
      option: ['없음',	'2,3일 이상',	'7일 이상',	'거의 매일']
    }
  },
  props: {
    diagnosticName: String,
  }
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
  background-color: #57687c;
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
  padding: 0.3rem;
  font-size: 1.9rem;
  line-height: 1;
}

.question {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 12rem;
  background-color: #B3D6FF;
  border-radius: 10px;
  padding: 1rem;
  font-size: xx-large;
  font-weight: bold;
  text-align: center;
}

.contour {
  border: 1px dashed #57687c;
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

</style>
