<template>
  <div class="select">
    <div class="select__wrap" :class="{ error: isError }">
      <span class="select__title" @click="showOption = !showOption">{{"Chọn tỉnh thành"
      }}</span>
      <transition name="fade">
        <div class="select__options" v-show="showOption">
          <div class="select__group">
            <slot name="option"></slot>
          </div>
          <div class="select__btn">
            <button @click="showOption = !showOption" :class="{ active: selected }" class="btn btn-ok">
              Đồng ý
            </button>
            <button @click="handleCancel" class="btn btn-cancel">Hủy</button>
          </div>
        </div>
      </transition>
     
    </div>
      

    <p class="select__error">{{ errorMessage }}</p>

    <MultiSelectResult v-show="!showOption" :selected="selected"></MultiSelectResult>
  </div>
</template>

<script>
import MultiSelectResult from './MultiSelectResult.vue'
export default {
  components: {
    MultiSelectResult,

  },
  data() {
    return {
      showOption: false,
      errorMessage: "",
    };
  },
  props: ["selected", "validFeild"],
  computed: {
    isError() {
      return !!this.errorMessage;
    },
  },
  watch: {
    validFeild() {
      if (!this.validFeild) {
        if (!this.selected) {
          this.handleValid();
        }
      }
    },
    showOption() {
      if (!this.showOption) {
        this.handleValid();
      }
    },
  },
  methods: {
    handleValid() {
      if (!this.selected) {
        this.errorMessage = "The field is required!";
      } else {
        this.errorMessage = "";
      }
    },
    handleCancel(){
      this.$store.commit('RESET_SELECTED');
      this.showOption = !this.showOption
    },
  },
};
</script>

<style scoped>
.select {
  text-align: left;
}
.select__label {
  font-size: 16px;
  color: #000;
  padding-bottom: 10px;
}

.select__wrap {
  border: 1px solid #ccc;
  border-radius: 6px;
  position: relative;
  cursor: pointer;
}
.select__wrap::after {
  content: "";
  display: block;
  border: 4px solid transparent;
  border-color: #000 transparent transparent transparent;
  position: absolute;
  top: calc(50% + 3px);
  right: 22px;
  transform: translateY(-50%);
}

.select .error {
  box-shadow: 0 0 4px #f4b6c1;
  border: 1px solid #aa4651;
}

.select__title {
  display: block;
  width: 100%;
  height: 100%;
  padding: 12px 12px;
}

.select__options {
  position: absolute;
  top: calc(100% + 1px);
  background-color: #fff;
  width: 100%;
  height: 360px;
  padding: 12px 0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  border-radius: 12px;
}

.select__group {
  overflow-y: auto;
  height: 290px;
}

.select__group::-webkit-scrollbar {
  width: 8px;
  height: 62px; 
  background: transparent;
  border-radius: 6px;
}
.select__group::-webkit-scrollbar-thumb {
  background: #dcdcdc;
}

.select__btn {
  width: 100%;
  margin: 16px;
}

.select__btn .btn {
  padding: 4px 19px;
  margin: 3px 16px;
  font-size: 16px;
  border-radius: 4px;
  border: none;
}

.select__btn .btn-ok {
  background: #dcdcdc;
  color: white;
    cursor: pointer;
  transition: all 0.5s
}

.active {
  background: #007bc3 !important;
}

.select__btn .btn-cancel {
  background: transparent;
  color: #007bc3;
  cursor: pointer;
  transition: all 0.5s
}

.select__btn .btn-cancel:hover{
  background: #dcdcdc;

}
.select__error {
  color: #aa4651;
  font-size: 12px;
  margin-top: 8px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.8s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.province-btn {
  color: red;
}
</style>
