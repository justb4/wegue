<template>
<!--remove <v-toolbar-side> -->
  <v-card class="wgu-helpwin">
    <v-toolbar :color="color" dark>
      <v-icon>{{ icon }}</v-icon>
      <v-toolbar-title v-if="windowTitle">{{ windowTitle }}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-icon @click="onWinXClose">close</v-icon>
    </v-toolbar>

    <v-card-title primary-title>
      <div>
        <h3 class="headline mb-0" v-if="textTitle">{{ textTitle }}</h3>
      </div>
    </v-card-title>
    <v-card-text>
      <div class="text--primary" v-html="htmlContent">
      </div>
    </v-card-text>

    <v-card-actions>
      <a class="info-link red--text darken3"
        :href="infoLinkUrl" target="_blank" v-if="infoLinkUrl">
        {{ infoLinkText || infoLinkUrl }}
        </a>
    </v-card-actions>
  </v-card>

</template>

<script>
  export default {
    props: {
      color: {type: String, required: false, default: 'red darken-3'},
      icon: {type: String, required: false, default: 'help'}
    },
    data () {
      return {
        show: false,
        windowTitle: this.$appConfig.modules['wgu-helpwin'].windowTitle,
        textTitle: this.$appConfig.modules['wgu-helpwin'].textTitle,
        htmlContent: this.$appConfig.modules['wgu-helpwin'].htmlContent,
        infoLinkUrl: this.$appConfig.modules['wgu-helpwin'].infoLinkUrl,
        infoLinkText: this.$appConfig.modules['wgu-helpwin'].infoLinkText
      }
    },
    methods: {
      onWinXClose: function () {
        this.$emit('winxclose');
      }
    }
  }
</script>

<style>
</style>
