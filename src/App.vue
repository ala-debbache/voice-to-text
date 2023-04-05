<script>
export default {
  data() {
    return {
      transcript: "",
      isRecording: false,
      sr: null,
    };
  },

  mounted() {
    const Recognition =
      window.SpeechRecognition || window.webkitSpeechRecognition;
    this.sr = new Recognition();
    this.sr.continuous = true;
    this.sr.interimResults = true;
    this.sr.lang = "ar-Dz";

    this.sr.onstart = () => {
      console.log("SR Started");
      this.isRecording = true;
    };

    this.sr.onend = () => {
      console.log("SR Stopped");
      this.isRecording = false;
    };

    this.sr.onresult = (evt) => {
      const t = Array.from(evt.results)
        .map((result) => result[0])
        .map((result) => result.transcript)
        .join("");

      this.transcript = t;
    };
  },

  methods: {
    ToggleMic() {
      if (this.isRecording) {
        this.sr.stop();
      } else {
        this.transcript = "";
        this.sr.start();
      }
    },
  },
};
</script>

<template>
  <div class="app">
    <div class="container">
      <h3 class="title">انقر ثم ابدأ في التحدث</h3>
      <button
        class="mic"
        :class="isRecording ? 'pause' : 'play'"
        @click="ToggleMic"
      >
        <vue-feather
          :type="isRecording ? 'pause' : 'play'"
          class="icon"
          size="42"
        ></vue-feather>
      </button>
      <div class="transcript" v-text="transcript"></div>
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira sans", sans-serif;
}
body {
  background: #051923;
  color: #f1f6f9;
}
.app {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container {
  width: 100%;
  min-height: 300px;
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
}
.title {
  font-size: 28px;
  margin-bottom: 20px;
}
.mic {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: none;
  margin-bottom: 20px;
}
.pause {
  background: #ffc43d;
}
.play {
  background: #00c49a;
}
.icon {
  color: #f1f6f9;
}
.transcript {
  direction: rtl;
  max-width: 50%;
  font-size: 18px;
}

@media only screen and (max-width: 720px) {
  .transcript {
    max-width: 80%;
  }
}
</style>
