<template>
  <div class="mx-auto max-w-5xl">
    <div class="p-2 text-sm bg-gray-100">
      Notice: Please use Google Chrome on Desktop or Android only. Other
      browsers are not supported.
    </div>
    <div class="p-4 bg-grey">
      <div class="container">
        <div class="card input-wrapper">
          <div class="from">
            <span class="heading">From :</span>
            <div class="dropdown-container" id="input-language">
              <LangSelect :title="toLang" @select:lang="selectToLang" />
            </div>
          </div>
          <div class="text-area">
            <textarea id="input-text" cols="30" rows="10" placeholder="Enter your text here" v-model="inputText"
              @keyup="translate"></textarea>
            <div class="chars"><span id="input-chars">{{ inputText.length }}</span> / 5000</div>
          </div>
        </div>

        <div class="center">
          <div class="swap-position" @click="swapLang">
            <ion-icon name="swap-horizontal-outline"></ion-icon>
          </div>
        </div>

        <div class="card output-wrapper">
          <div class="to">
            <span class="heading">To :</span>
            <div class="dropdown-container" id="output-language">
              <LangSelect :title="fromLang" @select:lang="selectFromLang" />
            </div>
          </div>
          <textarea id="output-text" cols="30" rows="10" placeholder="Translated text will appear here" disabled
            v-model="outputText"></textarea>
        </div>
      </div>

      <div class="speech-row">
        <div class="speech-btn" v-if="!listening">
          <button class="
            bg-blue-500
            hover:bg-blue-700
            text-white
            font-bold
            py-2
            px-4
            rounded
          " @click="send()">
            Speech message
          </button>
        </div>
        <div class="speech-btn" v-else>
          Speak your message...
          <button class="
            border border-blue-500
            hover:bg-blue-700
            hover:text-white
            py-2
            px-4
            rounded
          " @click="stop()">
            Stop listening
          </button>
        </div>
        <div class="speech-btn">
          <button class="
            bg-green-500
            hover:bg-green-700
            text-white
            font-bold
            py-2
            px-4
            mt-2
            rounded
          " @click="setStop()">
            non stop {{ nonstop }}
          </button>
        </div>
        <div class="speech-btn">
          <button class="
            bg-purple-500
            hover:bg-purple-700
            text-white
            font-bold
            py-2
            px-4
            mt-2
            rounded
          " @click="textToSpeech()">
            translator speech
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onUpdated, ref } from "vue";
import { langs } from "./langs";
import LangSelect from "./component/lang-select.vue"

declare var webkitSpeechRecognition: typeof SpeechRecognition;

export default defineComponent({
  name: "App",
  components: { LangSelect },
  setup() {
    const name = ref("speecher");
    const listening = ref(false);
    const draft = ref("");
    const nonstop = ref(false)
    const inputText = ref("");
    const outputText = ref("");
    const toLang = ref("ko");
    const fromLang = ref("en")
    const darkmode = ref(false)
    const sr2 = new webkitSpeechRecognition();
    const ssu = new SpeechSynthesisUtterance();
    sr2.lang = toLang.value;
    sr2.continuous = true;
    sr2.interimResults = true;

    const setStop = () => {
      nonstop.value = !nonstop.value;
    }
    const send = async () => {
      listening.value = true;
      sr2.lang = toLang.value;

      sr2.onresult = async function (event: any) {
        var finalTranscript = "";
        var interimTranscript = "";
        for (var i = event.resultIndex; i < event.results.length; ++i) {
          if (event.results[i].isFinal) {
            finalTranscript += event.results[i][0].transcript;
          } else {
            interimTranscript += event.results[i][0].transcript;
          }
        }
        draft.value = interimTranscript;
        if (finalTranscript) {
          inputText.value = finalTranscript;
          translate()
          if (nonstop.value == false) {
            sr2.stop();
          }
        }
      };
      sr2.onerror = () => {
        listening.value = false;
      };
      sr2.onend = () => {
        listening.value = false;
      };
      sr2.start();
    };
    const stop = () => {
      sr2.stop();
      listening.value = false;
    };

    const selectToLang = (value: string) => {
      toLang.value = value;
    }

    const selectFromLang = (value: string) => {
      fromLang.value = value;
      translate()
    }

    const translate = () => {
      const input = inputText.value;
      if (!input || input.length > 5000 || input.length == 0) {
        return;
      }

      const url = `https://translate.googleapis.com/translate_a/single?client=gtx&sl=${toLang.value}&tl=${fromLang.value}&dt=t&q=${encodeURI(
        inputText.value,
      )}`;

      fetch(url)
        .then((response) => response.json())
        .then((json) => {
          console.log(json);
          outputText.value = json[0].map((item: any) => item[0]).join("");
        })
        .catch((error) => {
          console.log(error);
        });
    }

    const swapLang = () => {
      let tmp = toLang.value;
      toLang.value = fromLang.value;
      fromLang.value = tmp;
    }

    const textToSpeech = () => {
      ssu.lang = fromLang.value
      ssu.pitch = 1
      ssu.rate = 1;
      ssu.text = outputText.value;

      window.speechSynthesis.speak(ssu)
    }

    return {
      name,
      nonstop,
      draft,
      listening,
      send,
      setStop,
      stop,
      langs,
      toLang,
      fromLang,
      selectToLang,
      selectFromLang,
      inputText,
      outputText,
      translate,
      darkmode,
      swapLang,
      textToSpeech
    };
  },
});
</script>
<style>
@import "./styles/style.css";

.speech-row {
  width: 100%;
  display: inline-block;
}

.speech-btn {
  display: inline-block;
  margin: 2%;
}
</style>
