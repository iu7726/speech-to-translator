<template>
  <div class="quick-row">
    <img class="quick-lang-btn" src="/korea.svg" @click="selectLang = 'ko'">
    <img class="quick-lang-btn" src="/english.svg" @click="selectLang = 'en'">
    <img class="quick-lang-btn" src="/japan.svg" @click="selectLang = 'ja'">
  </div>
  <select v-model="selectLang" class="dropdown-toggle">
    <option v-for="ln of languages" :key="ln.no" class="option" :value="ln.code">
      {{ ln.name }}
    </option>
  </select>
</template>

<script lang="ts">
import { languages } from "../languages";

export default {
  name: 'LangSelect',
  props: {
    title: {
      type: String,
      default: 'auto'
    }
  },
  data() {
    return {
      languages: languages,
      selectLang: this.title
    }
  },
  watch: {
    selectLang() {
      this.$emit('select:lang', this.selectLang)
    }
  }
}
</script>

<style>
.quick-row {
  width: 100%;
}

.quick-lang-btn {
  display: inline-block;
  width: 20%;
  margin: 3%;
  cursor: pointer;
}

.quick-lang-btn:hover {
  border: 2px solid #000;
  transform: scale(125%);
  transition-duration: 0.5s;
}
</style>