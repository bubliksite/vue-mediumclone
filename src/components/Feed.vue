<template>
  <div>
    <div v-if="isLoading">Loading...</div>
    <div v-if="error">Something wrong</div>
    <div v-if="feed">
      <div
        class="article-preview"
        v-for="(article, index) in feed.articles"
        :key="index"
      >
        <div class="article-meta">
          <router-link
            :to="{name: 'userProfile', params: {slug: article.author.username}}"
          >
            <img :src="article.author.image" :alt="article.author.username" />
          </router-link>
          <div class="info">
            <router-link
              class="author"
              :to="{
                name: 'userProfile',
                params: {slug: article.author.username},
              }"
            >
              {{ article.author.username }}
            </router-link>
            <span class="date">{{ article.createdAt }}</span>
          </div>
          <div class="pull-xs-right">ADD TO FAVORITES</div>
        </div>
        <router-link
          :to="{name: 'article', params: {slug: article.slug}}"
          class="preview-link"
        >
          <h1>{{ article.title }}</h1>
          <p>{{ article.description }}</p>
          <span>Read more...</span>
          TAGLIST
        </router-link>
      </div>
      PAGINATION
    </div>
  </div>
</template>

<script>
  import {actionTypes} from '../store/modules/feed'
  import {mapState} from 'vuex'

  export default {
    name: 'AppFeed',
    props: {
      apiUrl: {
        type: String,
        required: true,
      },
    },
    computed: {
      ...mapState({
        isLoading: (state) => state.feed.isLoading,
        feed: (state) => state.feed.data,
        error: (state) => state.feed.error,
      }),
    },
    mounted() {
      this.$store.dispatch(actionTypes.getFeed, {apiUrl: this.apiUrl})
    },
  }
</script>