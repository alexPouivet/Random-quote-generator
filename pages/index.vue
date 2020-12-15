<template>
  <div id="template">
    <header>
      <div class="button">
        <p @click="callRandomQuote()">random</p>
        <i class="material-icons">autorenew</i>
      </div>
    </header>
    <main>
      <div class="randomQuote" v-if="randomQuote">
        <div class="quote">
          <div class="bar"></div>
          <p>{{randomQuote.quoteText}}</p>
        </div>
        <div class="quote-author" @click="quotesAuthor(randomQuote.quoteAuthor)">
          <div>
            <p class="quote-author-name">{{randomQuote.quoteAuthor}}</p>
            <p class="quote-author-genre">{{randomQuote.quoteGenre}}</p>
          </div>
          <i class="material-icons">arrow_forward</i>
        </div>
      </div>
      <div class="authorQuotes" v-if="authorQuotes">
        <h2>{{authorName}}</h2>
        <div class="quote" v-for="(quote, index) in authorQuotes" :key="index">
          <div class="bar"></div>
          <p>{{quote.quoteText}}</p>
        </div>
      </div>
    </main>
    <footer>
      <p>Alexandre Pouivet @ DevChallenges.io</p>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      randomQuote : null,
      authorQuotes: null,
      authorName : null
    }
  },
  async asyncData({ $axios }) {
    const {data} = await $axios.$get('quotes/random')

    return {
      randomQuote: data[0]
    }
  },
  methods: {
    async callRandomQuote() {
      const {data} = await this.$axios.$get('quotes/random')

      this.randomQuote = data[0]
      this.authorQuotes = null
      this.authorName = null
    },
    async quotesAuthor(author) {
      const {data} = await this.$axios.get(`quotes?author=` + author)

      this.authorQuotes = data.data
      this.authorName = author
      this.randomQuote = null
    }
  }
}
</script>
