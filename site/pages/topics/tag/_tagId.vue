<template>
  <section class="main">
    <top-notice />
    <div class="container main-container is-white left-main">
      <div class="left-container">
        <topics-nav :nodes="nodes" />
        <topic-list :topics="topicsPage.results" :show-ad="true" />
        <pagination
          :page="topicsPage.page"
          :url-prefix="'/topics/' + tag.tagId + '?p='"
        />
      </div>
      <topic-side />
    </div>
  </section>
</template>

<script>
import TopicSide from '~/components/TopicSide'
import TopicsNav from '~/components/TopicsNav'
import TopicList from '~/components/TopicList'
import Pagination from '~/components/Pagination'
import TopNotice from '~/components/TopNotice'

export default {
  components: {
    TopicSide,
    TopicsNav,
    TopicList,
    Pagination,
    TopNotice
  },
  async asyncData({ $axios, params, query }) {
    const [tag, user, nodes, topicsPage] = await Promise.all([
      $axios.get('/api/tag/' + params.tagId),
      $axios.get('/api/user/current'),
      $axios.get('/api/topic/nodes'),
      $axios.get('/api/topic/tag/topics', {
        params: {
          tagId: params.tagId,
          page: query.p || 1
        }
      })
    ])
    return {
      tag,
      user,
      nodes,
      topicsPage
    }
  },
  head() {
    return {
      title: this.$siteTitle(this.tag.tagName + ' - 话题'),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.$siteDescription()
        },
        { hid: 'keywords', name: 'keywords', content: this.$siteKeywords() }
      ]
    }
  }
}
</script>

<style lang="scss" scoped></style>
