<template>

  <div class="title">
    <sapn>{{diagnosticName}}</sapn>
  </div>
  <div class="menu-grid">
    <div class="questionWrap">
      <div class="questionList"></div>
    </div>
  </div>

</template>

<script>

export default {
  name: 'DiagnosticsPage',
  props: {
    diagnosticName: String
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

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

.questionWrap {
  display: flex;
  flex-direction: column;
}

.questionList {
  border-radius: 10px;
  background-color: #E1E2E1;
}

.questionList:hover {
  background-color: #afb0af;
}

</style>
