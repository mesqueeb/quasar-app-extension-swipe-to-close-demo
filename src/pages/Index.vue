<template>
  <q-page padding class="flex flex-center">
    <div class="_demo flex flex-start">
      <div class="q-pt-lg q-mr-sm">
        <p>Set your browser to "mobile view" to have a better idea.</p>
        <h5 class="q-my-sm">Dialog settings</h5>
        <div>Currently you can only drag down, so set dialog position to "bottom" or no position.</div>
        <div class="q-gutter-sm">
          <QRadio v-model="position" val="top" label="top" disable />
          <QRadio v-model="position" val="left" label="left" disable />
          <QRadio v-model="position" val="right" label="right" disable />
        </div>
        <div class="q-gutter-sm">
          <QRadio v-model="position" val="bottom" label="bottom" />
          <QRadio v-model="position" :val="undefined" label="no position" />
        </div>

        <div class="q-mt-sm">Currently you cannot use this with "maximized"</div>
        <QToggle label="With scrollable content" v-model="scroll" :disable="scrollDisabled" />
        <QToggle label="Maximized" v-model="maximized" disable />
        <h5 class="q-my-sm">Try it out</h5>
        <div class="q-my-md">
          <QBtn color="accent" @click="showDialog = true">Show dialog</QBtn>
          <QToggle color="accent" v-model="turnExtOn" :label="turnExtOn ? 'Extension enabled' : 'Extension disabled'" />
        </div>
      </div>
      <!-- <THE DIALOG> -->
      <QDialog
        v-model="showDialog"
        :maximized="maximized"
        :position="position"
      >
        <q-swipe-to-close v-if="turnExtOn" v-model="showDialog">
          <div
            class="_dialog-contents"
            v-html="dialogInnerHtml"
          ></div>
        </q-swipe-to-close>
        <div
          v-if="!turnExtOn"
          class="_dialog-contents"
          v-html="dialogInnerHtml"
        ></div>
      </QDialog>
      <!-- </THE DIALOG> -->
      <div class="_template flex">
        <h5 class="q-my-xs full-width">Your template</h5>
        <textarea class="_pre bg-grey-2" v-model="dialogOuterHtml" />
      </div>
    </div>
    <img class="_bg-logo" alt="Quasar logo" src="~assets/quasar-logo-full.svg">
  </q-page>
</template>

<style lang="stylus">

._dialog-contents
  background white
  padding 5rem

._pre
  overflow scroll
  padding 1rem
  border solid darkgray thin
  max-width 350px
  min-height 280px
  flex 1

._demo
  position relative
  z-index 10
  background alpha(white, .9)
  padding 10px

._bg-logo
  position fixed

</style>

<script>
export default {
  name: 'PageIndex',
  data () {
    const lorem = `<p>"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."</p>`
    return {
      turnExtOn: true,
      showDialog: false,
      position: 'bottom',
      maximized: false,
      scroll: false,
      scrollDisabled: false,
      text: {
        noScroll: `
          <div>I'm a dialog!</div><div>Drag me down!</div>
          <div class="q-mt-xl text-bold">If you drag me ＜ 25% (of dialog height) I will bounce back!</div>
        `,
        scrollMax: `
          <h5>In a maximized modal you can drag to close anywhere!</h5>
          ${lorem}
          <h5>Even if you are scrolled down you can drag to close</h5>
        `,
        scrollMin: `
          <h5>If you Scroll down drag to close will be disabled. ↓↓↓</h5>
          ${lorem}
          <h5>Scroll up again and you will be able to drag to close! ↑↑↑</h5>
        `,
        lorem
      }
    }
  },
  watch: {
    maximized (to, from) {
      if (to === true) {
        this.scroll = false
        this.scrollDisabled = true
      }
      if (to === false) {
        this.scrollDisabled = false
      }
    }
  },
  computed: {
    dialogInnerHtml () {
      const t = this.text
      if (this.scroll && this.maximized) {
        return t.scrollMax + t.lorem + t.lorem + t.lorem
      }
      if (this.scroll && !this.maximized) {
        return t.scrollMin + t.lorem + t.lorem + t.lorem
      }
      if (!this.scroll) {
        return t.noScroll
      }
      return ``
    },
    dialogOuterHtml () {
      const position = this.position ? `\n:position="${this.position}"` : ''
      const maximized = this.maximized ? `\nmaximized` : ''
      return `<q-dialog
  v-model="showDialog"${position}${maximized}
>
  <q-swipe-to-close v-model="showDialog">
    <div>
      My content
    </div>
  </q-swipe-to-close>
</q-dialog>`
    }
  }
}
</script>
