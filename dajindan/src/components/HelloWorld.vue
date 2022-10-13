<template>

  <div class="jindan-wrap">
    <div class="inner-wrap">
      <div class="title">
        <sapn>다-진단 종합 자가 진단</sapn>
      </div>
      <div class="menu-grid">
        <div class="tiles" v-for="(item, i) in diagnosticList" :key = i>
          <span>{{ item }}</span>
        </div>
      </div>
    </div>
  </div>

</template>

<script>

export default {
  name: 'HelloWorld',
  data() {
    return {
      diagnosticList : [ "PHQ-9(우울증)", "조울증", "조현병", "PTSD", "알코올중독", "ADHD", "", "", "" ]
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
  padding: 1rem;
  border-radius: 10px;
  background-color: #E1E2E1;
  cursor: pointer;
  font-size: x-large;
  font-weight: bold;
}

.tiles:hover {
  background-color: #afb0af;
}

</style>
