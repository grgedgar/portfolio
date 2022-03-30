<template>
  <div>
    <textarea v-model="sourceText" placeholder="Введіть текст для перекладу" type='text' ref='sourceText' defaultValue="" v-on:keyup="translateText"></textarea>
    {{translatedText}}
  </div>
</template>

<script>


export default {
  name: 'app',
  methods: {
    translateText() {
      const encodedParams = new URLSearchParams();
      encodedParams.append('q', this.sourceText);
      encodedParams.append('target', 'en');
      encodedParams.append('source', 'uk');

      const options = {
        method: 'POST',
        url: 'https://google-translate1.p.rapidapi.com/language/translate/v2',
        headers: {
          'content-type': 'application/x-www-form-urlencoded',
          'X-RapidAPI-Host': 'google-translate1.p.rapidapi.com',
          'X-RapidAPI-Key': '19f5154d4amsh655ab22a91471aap154ab6jsn2a97b243d394',
        },
        data: encodedParams,
      };

      this.$axios.request(options).then((response) => {
        this.translatedText = response.data.data.translations[0].translatedText;
      }).catch((error) => {
        console.error(error);
      });
    },
  },
  data() {
    return {
      translatedText: '',
      sourceText: '',
    };
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
