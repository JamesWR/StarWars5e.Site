<script lang="ts">
  import { Component, Prop, Vue, Watch } from 'vue-property-decorator'
  import { namespace } from 'vuex-class'
  import VueMarkdownWithAnchors from '@/components/VueMarkdownWithAnchors.vue'
  import Loading from '@/components/Loading.vue'

  const blobsModule = namespace('blobs')

  @Component({
    components: {
      VueMarkdownWithAnchors,
      Loading
    }
  })
  export default class HivesChapter extends Vue {
    @blobsModule.State hivesBlobs!: { [key: string]: string }
    @blobsModule.Action fetchHivesBlob!: (chapter: string) => void

    @Prop(String) readonly chapter!: string

    chapterMap: { [key: string]: string } = {
      downtime: 'Downtime',
      factionsAndMembership: 'Factions and Membership',
      enhancedItems: 'Enhanced Items',
      modifiableItems: 'Modifiable Items',
      cyberneticAugmentations: 'Cybernetic Augmentations',
      droidCustomizations: 'Droid Customizations',
      toolProficiencies: 'Tool Proficiencies'
    }

    created () {
      this.fetchHivesBlob(this.blobName)
    }

    @Watch('$route')
    fetchData () {
      this.fetchHivesBlob(this.blobName)
    }

    get blobName () {
      return this.chapterMap[this.chapter] || 'Downtime'
    }

    get blob () {
      const blob = this.hivesBlobs[this.blobName]
      return blob && blob.replace(/\ufffd/g, ' - ')
    }
  }
</script>

<template lang="pug">
  div(v-if="blob")
    VueMarkdownWithAnchors(:source="blob").text-xs-left
    slot
  Loading(v-else)
</template>