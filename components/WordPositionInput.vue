<template>
  <form class="input-group mb-3" @submit.prevent="onSubmit">
    <label class="input-group-prepend" for="inlineFormCustomSelectPref">
      <span class="input-group-text">{{pinyin}}</span>
    </label>
    <input
      type="text"
      class="form-control mb-2 mr-sm-2"
      placeholder="請輸入起始時間  e.g. 11:12.3"
      v-model="startSecStr"
    />
    <button type="submit" class="btn btn-dark mb-2" :disabled="saying">更新單字時間</button>
  </form>
</template>
<script>
export default {
  props: ["pinyin", "startSec", "duration", "saying"],
  data() {
    return {
      tSec: 0
    };
  },
  computed: {
    startSecStr: {
      get() {
        return this.startSec
          ? `${Math.floor(this.startSec / 60)}:${Math.round(
              (this.startSec % 60) * 10
            ) / 10}`
          : "";
      },
      set(val) {
        const tss = val.split(":");
        const minute = parseInt(tss[0]);
        const seconds = parseFloat(tss[1]);
        this.tSec = minute * 60 + seconds;
      }
    }
  },
  methods: {
    onSubmit() {
      this.$emit("submit", {
        pinyin: this.pinyin,
        startSec: this.tSec,
        duration: this.duration
      });
    }
  },
  mounted() {
    this.tSec = this.startSec;
  }
};
</script>