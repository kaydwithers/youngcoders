<template lang="pug">
.blog-slug
  img.vh-25.h-auto-l.of-c(
    :src="post.fields.heroImage.fields.file.url + '?fit=scale&w=350&h=196'"
    :srcset="`${post.fields.heroImage.fields.file.url}?w=350&h=87&fit=fill 350w, ${post.fields.heroImage.fields.file.url}?w=1000&h=250&fit=fill 1000w, ${post.fields.heroImage.fields.file.url}?w=2000&h=500&fit=fill 2000w`"
    size="100vw"
    :alt="post.fields.heroImage.fields.description"
  )

  .bg-white
    .mw9.center.ph3.ph4-ns.pv6
      .w-100

        .p6(v-text='( new Date(post.fields.publishDate)).toDateString()')

        h1.db.f3.f2-ns.lh-copy.fw6.mb4(
          :to="{ name: 'blog-slug', params: { slug: post.fields.slug }}"
        ) {{ post.fields.title }}

        p.p5 {{post.fields.body}}

</template>

<script>
import {createClient} from '~/plugins/contentful.js'
import appHeading from '~/components/atoms/app-heading'
import appParagraph from '~/components/atoms/app-paragraph'
import Navigation from '~/components/navigation.vue'

const client = createClient()

export default {
  asyncData ({ env, params }) {
    return client.getEntries({
      'content_type': env.CTF_BLOG_POST_TYPE_ID,
      'fields.slug': params.slug
    }).then(entries => {
      return {
        post: entries.items[0]
      }
    })
    .catch(console.error)
  },

  components: {
    appHeading,
    appParagraph,
    Navigation
  }
}
</script>

<style lang="scss">
.blog-slug {

  h1, h2, h3, h4, h5, h6 {
    color: black;
    margin-top: 3rem;
  }

}
</style>
