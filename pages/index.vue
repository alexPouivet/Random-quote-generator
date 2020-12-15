<template>
  <div id="template">
    <header>
      <button type="button" name="button" @click="callRandomQuote()">random</button>
    </header>
    <main>
      <div>
        <h1>Random quote generator</h1>
        <div class="randomQuote" v-if="randomQuote">
          <div class="quote">
            <p>{{randomQuote.quoteText}}</p>
          </div>
          <div class="quote-author">
            <button type="button" name="button" @click="quotesAuthor(randomQuote.quoteAuthor)">All quote from {{randomQuote.quoteAuthor}}</button>
            <p>{{randomQuote.quoteGenre}}</p>
          </div>
        </div>
        <div class="authorQuotes" v-if="authorQuotes">
          <h3>Author Quotes</h3>
          <h2>{{authorName}}</h2>
          <div class="quote" v-for="(quote, index) in authorQuotes" :key="index">
            <p>{{quote.quoteText}}</p>
          </div>
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
