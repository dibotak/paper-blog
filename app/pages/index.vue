<script setup lang="ts">
const { data: articles } = await useAsyncData('articles', () =>
  queryCollection('articles')
    .where('draft', '=', false)
    .order('date', 'DESC')
    .all()
)

useHead({
  title: 'Journal — Paper',
})
</script>

<template>
  <div class="container">
    <header class="page-header">
      <h1>The Journal</h1>
      <p>Thoughts, stories, and ideas written on paper.</p>
    </header>

    <ul v-if="articles && articles.length" class="article-list">
      <li v-for="article in articles" :key="article.path" class="article-item">
        <h2>
          <NuxtLink :to="article.path">{{ article.title }}</NuxtLink>
        </h2>
        <p v-if="article.description" class="excerpt">
          {{ article.description }}
        </p>
        <div class="meta">
          <time v-if="article.date" :datetime="article.date">{{ article.date }}</time>
          <span v-if="article.tags && article.tags.length" class="tags">
            <span v-for="tag in article.tags" :key="tag" class="tag">{{ tag }}</span>
          </span>
        </div>
      </li>
    </ul>

    <div v-else class="empty-state">
      <p>No articles yet. Check back soon.</p>
    </div>
  </div>
</template>
