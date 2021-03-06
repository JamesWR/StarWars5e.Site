<script lang="ts">
  import { Component, Prop, Vue, Watch } from 'vue-property-decorator'
  import { namespace } from 'vuex-class'
  import RoutesList from '@/components/RoutesList.vue'
  import CardSet from '@/components/CardSet.vue'
  import { omit } from 'lodash'

  const uiModule = namespace('ui')

  @Component({
    components: {
      RoutesList,
      CardSet
    }
  })
  export default class HomePage extends Vue {
    @uiModule.State isDarkSide!: boolean
    @uiModule.Action toggleDarkSide!: (value: boolean) => Promise<void>

    toggleDarkSideTheme () {
      this.$vuetify.theme.dark = !this.isDarkSide
      this.toggleDarkSide(!this.isDarkSide)
    }

    get socialLinks () {
      return [
        {
          href: 'https://www.reddit.com/r/sw5e',
          icon: 'fab fa-reddit',
          title: 'Reddit'
        },
        {
          href: 'https://discord.gg/zYcPYTu',
          icon: 'fab fa-discord',
          title: 'Discord'
        },
        {
          href: 'https://twitter.com/Galiphile',
          icon: 'fab fa-twitter',
          title: 'Twitter'
        },
        {
          href: 'https://www.patreon.com/sw5e',
          icon: 'fab fa-patreon',
          title: 'Patreon'
        }
      ]
    }

    get books () {
      return [
        {
          to: 'rules/phb',
          title: 'Player\' Handbook',
          image: 'phb_cover'
        },
        {
          to: 'rules/snv',
          title: 'Monster Manual',
          image: 'sav_cover'
        },
        {
          to: 'rules/sotg',
          title: 'Starships of the Galaxy',
          image: 'sotg_cover'
        },
        {
          to: 'rules/wh',
          title: 'Wretched Hives',
          image: 'wh_cover'
        }
      ]
    }

    get themeToggleColor () {
      return this.isDarkSide ? this.$vuetify.theme.themes.light.secondary : 'primary'
    }
  }
</script>

<template lang="pug">
  div
    h1 Star Wars 5e
    CardSet(:cards="books")
      template(v-slot="{ card }")
        v-img(:src="require(`@/assets/${card.image}.jpg`)", :class="$style.image")
    p
      | What is Star Wars 5e? It's a full conversion for Dungeons and Dragon Fifth Edition to the Star Wars universe.
      | Here you can find rules on creating a lightsaber-swinging jedi sentinel, a devil-may-care smuggler operative, or
      | a droid-hacking engineer. There's also a full collection of Star Wars monsters, NPCs, and constructs to fight
      | against. Ready to take to the stars? All the rules on piloting and customizing your very own spaceship are here
      | too. If you want to help contribute to this conversion, join one of the active communities at the links below!
    div(:class="$style.routes").mb-5
      a(v-for="{ href, icon, title, author } in socialLinks", :key="title", :href="href", target="_blank").ma-2
        v-btn(light)
          v-icon(:color="title").mr-3 {{ icon }}
          | {{ title }}
    v-btn(:color="themeToggleColor", @click="toggleDarkSideTheme") Join the {{ isDarkSide ? 'Light' : 'Dark' }} Side
</template>

<style module lang="scss">
  @import "src/assets/styles/colors.scss";

  .image {
    background-color: $black;
  }

  .routes {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }

  .darkSideSwitch {
    justify-content: center;
  }
</style>
