<template lang="pug">
.contact
  app-hero(:hero='hero' text='Contact us' hero-small)
  .bg-white
    .mw9.center.ph3.ph4-ns.pv6
      .w-100
        app-heading(size='2' :text='page.fields.heading')
        p.p4.mb0 {{page.fields.content}}

  .bg-lg-mirage
    .mw9.center.ph3.ph4-ns.pv6
      .w-100
        form.white-90(
          action='https://formspree.io/kaydwithers@gmail.com' 
          method='POST' 
          name='contact' 
        )

          // input(type='hidden' name='_next' value='/success')
          input(type='hidden' name='_subject' value='New Young Coders Form Submission!')
          input(type='hidden' name='_cc' value='hello@youngcodersau.com,kayd.withers@isobar.com')

          .dn
            input(type='text' name='_gotcha')


          .mb4.f5
            label.db.mb3(for='name') Name
            input#name.input-reset.ba.b--black-20.pa3.db.w-100(
              type='text' name='name' placeholder='Your name' required=''
            )

          .mb4.f5
            label.db.mb3(for='_replyto') Email
            input#email.input-reset.ba.b--black-20.pa3.db.w-100(
              type='email' name='_replyto' placeholder='Your email' required=''
            )
            //small.f6.black-60.db.mt3(id='name-desc') Helper text for the form control.

          .mb4.f5
            label.db.mb3(for='message') Message 
              // span.normal.white-60 (optional)
            textarea#message.input-reset.ba.b--black-20.pa3.db.w-100.h4(
              name='message' placeholder='Your message' required=''
            )

          input.f4.pointer.pa3.b0.dib.bn.br3.black(
            type="submit" class="form-control submit" value="Send"
          )

  twitter-head-card

</template>

<script>
import {createClient} from '~/plugins/contentful.js'
import appHeading from '~/components/atoms/app-heading'
import appHero from '~/components/organisms/app-hero'
import appParagraph from '~/components/atoms/app-paragraph'
import twitterHeadCard from '~/components/atoms/twitter-head-card'

const client = createClient()

export default {
  name: 'contact',

  head: {
    title: 'Young Coders ʕ•ᴥ•ʔ — Contact',
    meta: [
      { hid: 'description', name: 'description', content: 'Contact' }
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
        page: page.items[3]
      }
    }).catch(console.error)
  },

  components: {
    appHeading,
    appHero,
    appParagraph,
    twitterHeadCard
  }
}
</script>

<style>

</style>
