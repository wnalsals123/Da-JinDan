<template>

  <div class="jindan-wrap">
    <div v-if="diagnostic === 'main'" class="inner-wrap">
      <div class = "title">
        <span>다-진단 종합 자가 진단</span>
      </div>
      <div class="menu-grid">
        <div class="tiles" v-for="(item, i) in diagnosticList" :key = i>
          <button @click="toDiagnosticsPage(item)">{{ item }}</button>
        </div>
      </div>
    </div>
    <DiagnosticsPage v-else class="inner-wrap" :diagnosticName="diagnostic" @setDiagnostic="setDiagnostic"></DiagnosticsPage>
    <CustomModal v-if="modalVis" @toWhere="toMain" @setModalHidden="setModalHidden" :moodalOption="moodalOption"></CustomModal>
  </div>

</template>

<script>

import DiagnosticsPage from './DiagnosticsPage.vue'
import CustomModal from './CustomModal.vue'

export default {
  name: 'MainPage',
  components: {
    DiagnosticsPage,
    CustomModal
  },
  data() {
    return {
      diagnosticList: [ "PHQ-9(우울증)", "조울증", "조현병", "PTSD", "알코올중독", "ADHD", "", "", "" ],
      diagnostic: "main",
      modalVis: false,
      moodalOption: {
        msg: '업데이트 예정입니다.',
        selection: [
          { buttonMsg: "기대돼요!", buttonAction: false },
        ]
      }
    }
  },
  methods: {
    setDiagnostic(diagnostic) {
      this.diagnostic = diagnostic
    },
    toDiagnosticsPage(item) {
      if(item === '') return
      if(item !== 'PHQ-9(우울증)') {
        this.modalVis = true
        return
      }      
      this.setDiagnostic(item)
    },
    toMain() {
      this.setDiagnostic("main")
    },
    setModalHidden(aciton) {
      this.modalVis = aciton
    }
  },
  props: {
    
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.jindan-wrap {
  display: flex;
  width: 70%;
  height: 85%;
  background-color: white;
  border-radius: 10px;
}

.inner-wrap {
  display: flex;
  flex-direction: column;
  margin: 1rem;
  flex-grow: 1;
  overflow-y: auto;
}

.title {
  border-radius: 10px;
  background-color: #2c3e50;
  padding: 1rem;
  color: white;
  text-align: center;
  font-size: xx-large;
  font-weight: bold;
  margin-bottom: 1rem;
}

.menu-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  flex-grow: 1;
}

.tiles {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.tiles button {
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

.tiles button:hover {
  background-color: #afb0af;
}

</style>
