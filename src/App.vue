<template>
  <div>
    <div class="container">
      <span>Translate from / Перекласти з</span>
      <select id="selectedSourceLanguage" @change="selectSourceLanguage()">
      <option value="auto">Auto detect</option>
      </select>
      &nbsp; &nbsp;
      <span>Translate to / Перекласти на</span>
      <select id="selectedTargetLanguage" @change="selectTargetLanguage()">
        <option value="uk">uk</option>
      </select>
    </div>
    <div class="container">
      <textarea v-model="sourceText" placeholder="Введіть текст для перекладу" type='text' ref='sourceText' defaultValue="" v-on:keyup="detectSourceLanguage"></textarea>
      <div class="translated-text-container">{{translatedText}}</div>
    </div>
    <div class="container">
      <div id="detected-language"></div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'app',
  mounted() {
    this.getLanguagesList();
  },
  methods: {
    selectSourceLanguage() {
      const e = document.getElementById('selectedSourceLanguage');
      this.selectedSourceLanguage = e.options[e.selectedIndex].text;
      this.translateText();
    },
    selectTargetLanguage() {
      const e = document.getElementById('selectedTargetLanguage');
      this.selectedTargetLanguage = e.options[e.selectedIndex].text;
      this.translateText();
    },

    detectSourceLanguage() {
      if (new Date() - this.lastCall < 1000) {
        return;
      }
      this.lastCall = new Date();

      if (!this.selectedSourceLanguage) {
        this.autoDetectLanguage();
        return;
      }
      const e = document.getElementById('selectedSourceLanguage');
      if (e.options[e.selectedIndex].text === 'Auto detect') {
        this.autoDetectLanguage();
      } else {
        this.translateText();
      }
    },

    translateText() {
      const encodedParams = new URLSearchParams();
      encodedParams.append('q', this.sourceText);
      encodedParams.append('target', this.selectedTargetLanguage);
      encodedParams.append('source', this.selectedSourceLanguage);

      const options = {
        method: 'POST',
        url: 'https://google-translate1.p.rapidapi.com/language/translate/v2',
        headers: {
          'content-type': 'application/x-www-form-urlencoded',
          'X-RapidAPI-Host': 'google-translate1.p.rapidapi.com',
          'X-RapidAPI-Key': '2501d822damsh1a5073c2a7f7b41p1aa7f6jsnbea5985bc7d0',
        },
        data: encodedParams,
      };

      this.$axios.request(options).then((response) => {
        this.translatedText = response.data.data.translations[0].translatedText;
      }).catch((error) => {
        console.error(error);
      });
    },

    getLanguagesList() {
      const options = {
        method: 'GET',
        url: 'https://google-translate1.p.rapidapi.com/language/translate/v2/languages',
        headers: {
          'X-RapidAPI-Host': 'google-translate1.p.rapidapi.com',
          'X-RapidAPI-Key': '2501d822damsh1a5073c2a7f7b41p1aa7f6jsnbea5985bc7d0',
        },
      };

      this.$axios.request(options).then((response) => {
        this.languagesList = response.data.data.languages;
        const selSource = document.getElementById('selectedSourceLanguage');
        const selTarget = document.getElementById('selectedTargetLanguage');
        this.languagesList.forEach((language) => {
          const optSource = document.createElement('option');
          optSource.value = this.languagesList.id;
          optSource.textContent += language.language;
          selSource.appendChild(optSource);
          const optTarget = document.createElement('option');
          optTarget.value = this.languagesList.id;
          optTarget.textContent += language.language;
          selTarget.appendChild(optTarget);
        });
      }).catch((error) => {
        console.error(error);
      });
    },

    autoDetectLanguage() {
      const encodedParams = new URLSearchParams();
      encodedParams.append('q', this.sourceText);

      const options = {
        method: 'POST',
        url: 'https://google-translate1.p.rapidapi.com/language/translate/v2/detect',
        headers: {
          'content-type': 'application/x-www-form-urlencoded',
          'X-RapidAPI-Host': 'google-translate1.p.rapidapi.com',
          'X-RapidAPI-Key': '2501d822damsh1a5073c2a7f7b41p1aa7f6jsnbea5985bc7d0',
        },
        data: encodedParams,
      };

      this.$axios.request(options).then((response) => {
        this.autoDetectedLanguage = response.data.data.detections[0][0].language;
        if (this.autoDetectedLanguage !== 'und') {
          this.selectedSourceLanguage = this.autoDetectedLanguage;
          document.getElementById('detected-language').innerHTML = `<i>Detected language — ${this.autoDetectedLanguage}</i>`;

          this.translateText();
        }
      }).catch((error) => {
        console.error(error);
      });
    },
  },
  data() {
    return {
      languagesList: [],
      selectedSourceLanguage: '',
      selectedTargetLanguage: 'uk',
      translatedText: '',
      sourceText: '',
      autoDetectedLanguage: '',
      lastCall: 0,
    };
  },
};
</script>

<style>
*{
  font-family: 'Verdana', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.container{
  text-align: center;
  margin-top: 5vh;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 10vh;
}
select {
  margin: 0 10px;
}
.translated-text-container{
  margin: 0 20px;
  padding: 5px;
  width: 30vh;
  height: 10vh;
  border: 2px solid #2c3e50;
}
</style>
