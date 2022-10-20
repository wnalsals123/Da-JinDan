<template>

  <div class="modalWrap">
    <div class="modalContentWrap">
      <div class="modalContent">
        <span>{{moodalOption.msg}}</span>
        <div class="modalButtonWrap">
          <button v-for="(선택지, i) in moodalOption.selection" :key="선택지"
            @click="buttonAction(선택지.buttonAction, i)">{{선택지.buttonMsg}}</button>
        </div>
      </div>
    </div>
  </div>

</template>

<script>

export default {
  name: 'CustomModal',
  data() {
    return {

    }
  },
  props: {
    moodalOption: Object
  },
  methods: {
    buttonAction(action, test) {
      console.log(this.moodalOption, action, test)
      if (this.moodalOption.msg === "결과를 확인하시겠습니까?") {
        if (action) {
          this.$emit('setResult', true)
          this.$emit('setModalHidden', false)
        } else {
          this.$emit('setModalHidden', false)
        }
        return
      }
      action ? this.$emit('toWhere') : this.$emit('setModalHidden', false)
    }
  },
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modalWrap {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgb(3, 24, 40, 0.5);
}

.modalContentWrap {
  width: 20%;
  background-color: white;
  border-radius: 10px;
}

.modalContent {
  padding: 1rem;
}

.modalContent span {
  white-space: pre-line;
}

.modalButtonWrap {
  display: flex;
  justify-content: flex-end;
}

.modalButtonWrap button {
  margin-left: 0.5rem;
}
</style>