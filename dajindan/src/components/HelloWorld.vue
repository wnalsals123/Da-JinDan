<template>

  <div class="jindan-wrap">
    <div v-if="diagnostic === 'main'" class="inner-wrap">
      <div class = "title">
        <sapn>다-진단 종합 자가 진단</sapn>
      </div>
      <div class="menu-grid">
        <div class="tiles" v-for="(item, i) in diagnosticList" :key = i>
          <button @click="toDiagnosticsPage(item)">{{ item }}</button>
        </div>
      </div>
    </div>
    <div v-else class="inner-wrap">
      <DiagnosticsPage :diagnosticName="diagnostic"></DiagnosticsPage>
    </div>
  </div>

</template>

<script>

import DiagnosticsPage from './DiagnosticsPage.vue'

export default {
  name: 'HelloWorld',
  components: {
    DiagnosticsPage
  },
  data() {
    return {
      diagnosticList : [ "PHQ-9(우울증)", "조울증", "조현병", "PTSD", "알코올중독", "ADHD", "", "", "" ],
      diagnostic: "main"
    }
  },
  methods: {
    setDiagnostic(string) {
      this.diagnostic = string
    },
    toDiagnosticsPage(item) {
      if(item === '') return
      this.setDiagnostic(item)
    }
  },
  props: {
    msg: String
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.jindan-wrap {
  display: flex;
  width: 70%;
  height: 80%;
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
  background-color: #57687c;
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
