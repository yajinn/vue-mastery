<template lang="pug">
.container
  .cloud-cover
  .wrapper
    div(v-if='ready')
      transition-group(name="nature" @after-enter="showContent" appear)
        img.sm-range(src="/images/img-sm-mountain.svg" key="sm-range")
        img.bk-cloud(src="/images/img-cloud-back.svg" key="bk-cloud")
        img.md-range(src="/images/img-md-mountain.svg" key="md-range")
        img.mid-cloud(src="/images/img-cloud-middle.svg" key="mid-cloud")
        img.lg-range(src="/images/img-lg-mountain.svg" key="lg-range")
        img.fr-cloud(src="/images/img-cloud-front.svg" key="fr-cloud")

      .title
        transition-group(name="thankyou" @after-enter="showCard")
          h1.headline(v-if='show' key="headline") Thank You
          p.copy(v-if='show' key="copy") You are now enrolled for Vue Mastery Free Weekend.

      transition(name="thankyou")
        .card(v-if="card")
          .card-body
            h4
              i.fa.fa-lock-open &nbsp;
              | All Courses Unlocked 
            p You have until February 24th at 11:59 PM EST (23:59) to take all the lessons you can.  
            p
              nuxt-link.button.primary(to='/courses') View Courses

            h4
              i.fa.fa-share-square &nbsp;
              | Don't Let your Friends Miss Out
            p Have any friends that'd appreciate free learning?  Share #[a(:href="link") this link] or hit a share button below to make sure they don't miss out.
              social-sharing(inline-template 
                  :url='link'
                  :title="description"
                  :description="description"
                  :quote="description"
                  twitter-user='vuemastery')
                .social-wrapper
                  network.button.primary.border.-has-icon(network='twitter')
                    i.fab.fa-twitter
                    span Twitter
                  network.button.primary.border.-has-icon(network='facebook')
                    i.fab.fa-facebook
                    span Facebook
                  network.button.primary.border.-has-icon(network='linkedin')
                    i.fab.fa-linkedin
                    span LinkedIn
                  network.button.primary.border.-has-icon(network='reddit')
                    i.fab.fa-reddit
                    span Reddit
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'page-thank-you-free-weekend',

  head () {
    return {
      title: 'Thank you | Vue Mastery'
    }
  },

  middleware: 'anonymous',

  data () {
    return {
      ready: false,
      show: false,
      card: false,
      link: 'https://www.vuemastery.com/free-weekend',
      description: 'Vue Mastery\'s free weekend is going on right now.  Don\'t miss out on these free Vue.js courses.'
    }
  },

  computed: {
    ...mapState({
      account: result => result.account.account
    })
  },

  mounted () {
    setTimeout(() => {
      this.ready = true
      if (this.account && !this.account.enrolledFreeWeekend) {
        // This is to ensure people that are just login are subscribed
        this.$store.dispatch('userEnrollFreeWeekend')
      }
    }, 500)
  },

  methods: {
    showContent () {
      this.show = true
    },
    showCard () {
      this.card = true
    }
  }
}
</script>

<style lang="stylus" scoped>
.container
  position relative
  overflow hidden
  margin-top -100px
  padding-top 100px
  background linear-gradient(to bottom right, #90C3D3, #EDFFED)

.cloud-cover
  display none

  +laptop-up()
    display block
    position absolute
    top -590px
    width 100%
    height 100%
    background-size cover
    background-position center top
    background-repeat no-repeat
    background-image url(/images/bkg-cloud-top.svg)

.wrapper
  position relative
  height 1050px

  // Media for iPhone 5/SE
  @media (max-width: 325px)
    height 1200px

  +desktop-up()
    height 890px

.button
  margin-bottom 0

>>>.social-wrapper .button
  margin 15px 21px 0px 0
  min-width 162px

img[class*="range"]
img[class*="cloud"]
  position absolute

.lg-range
  width 1147px
  height 672px
  bottom 0
  right 50%
  transform translateX(50%)
  +laptop-up()
    right -265px
    transform none

.md-range
  display none
  +laptop-up()
    display block
    width 865px
    height 487px
    bottom 0
    right 275px

.sm-range
  display none
  +laptop-up()
    display block
    width 653px
    height 440px
    right 643px
    bottom 0

.bk-cloud
  display none
  +laptop-up()
    display block
    width 550px
    height 56px
    right 600px
    top 50%

.mid-cloud
  display none
  +laptop-up()
    display block
    width 747px
    height 76px
    right 450px
    top 60%

.fr-cloud
  width 2861px
  height 503px
  right -626px
  bottom 70px

.title
  position relative
  top 0
  bottom 0
  z-index 1
  width 100%

  +laptop-up()
    margin-top $vertical-space*1.5
    padding-left 5%
    width 45%
    z-index 0

.headline
  position absolute
  top 0
  color #FFFFFF
  font-size 60px
  +tablet-up()
    font-size 80px

.copy
  position absolute
  top 90px
  +tablet-up()
    top 120px

.start
  position absolute
  top 140px
  +tablet-up()
    top 170px

.card
  position absolute
  box-shadow none
  width 90%
  left 5%
  top 240px
  transform-origin top center

  +tablet-up()
    top 280px

  +laptop-up()
    top 0
    width 45%
    left 50%
    margin-top $vertical-space
    transform-origin center left

  +desktop-up()
    width 42%

.card-body
  padding 22px 40px
  h4
    color $secondary-color
  i
    color $primary-color

  +tablet-up()
    p
      padding-left 28px

.nature-enter-active
  transition opacity .3s ease-in

  &.lg-range
    transition transform 1.5s ease-in-out

  &.fr-cloud
    transition transform 2s ease-in-out

  +laptop-up()
    &.sm-range
      transition transform 1s ease-in-out

    &.md-range
      transition transform 1.3s ease-in-out

    &.bk-cloud
      transition transform 2.8s ease-in-out

    &.mid-cloud
      transition transform 2.5s ease-in-out

.nature-enter, .nature-leave-to
  opacity 0
  transform translateX(100%)

  &.fr-cloud
    transform translateX(25%)

.thankyou-enter-active
  &.headline
    transition transform 0.7s ease-out

  &.copy
    transition transform 0.9s ease-out

  &.start
    transition transform 1.1s ease-out

  &.card
    transition transform .5s ease-in-out

.thankyou-enter, .thankyou-leave-to
  transform translateY(800px)

  &.card
    transform scale(0)

</style>

