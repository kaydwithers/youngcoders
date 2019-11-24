<template lang="pug">
.about
  app-hero(:hero='hero' text='About us' hero-small)
  .bg-white
    .mw9.center.ph3.ph4-ns.pv6
      .w-100
        app-heading(size='2' :text='page.fields.heading')
        .p4.mb0 {{ page.fields.content }}

  .bg-lg-pink
    .mw9.center.ph3.ph4-ns.pv6
      .w-100
        app-avatars

  // app-test

  twitter-head-card

</template>

<script>
import {createClient} from '~/plugins/contentful.js'
import appAvatars from '~/components/organisms/app-avatars'
import appHeading from '~/components/atoms/app-heading'
import appHero from '~/components/organisms/app-hero'
import appParagraph from '~/components/atoms/app-paragraph'
import appTest from '~/components/app-test'
import twitterHeadCard from '~/components/atoms/twitter-head-card'

const client = createClient()

export default {
  name: 'about',

  head: {
    title: 'Young Coders ʕ•ᴥ•ʔ — About',
    meta: [
      { hid: 'description', name: 'description', content: 'About' }
    ]
  },

  asyncData ({ env }) {
    return Promise.all([
      // fetch the owner of the blog
      client.getEntries({
        'content_type': process.env.CTF_PAGE_ID,
        order: '-sys.createdAt'
      }),
      client.getEntries({
        'content_type': process.env.CTF_HERO_ID,
        order: '-sys.createdAt'
      })
    ]).then(([hero, page]) => {
      // return data that should be available
      // in the template
      return {
        hero: hero.items[0],
        page: page.items[6]
      }
    }).catch(console.error)
  },

  components: {
    appAvatars,
    appHeading,
    appHero,
    appParagraph,
    appTest,
    twitterHeadCard
  }
}
</script>

<style>

</style>
