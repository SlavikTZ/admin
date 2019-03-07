<template lang="pug">
  v-card
    v-card-title.grey.lighten-4
      h3.headline.mb-0 {{ $t('Dashboard') }}
    v-divider
    v-card-text
      v-alert(v-model="alert",
      dismissible,
      type="success") test
</template>

<script>
export default {
  name: 'home-view',
  data(){
    return {
      alert:true
    }
  },
  metaInfo () {
    return { title: this.$t('Dashboard') }
  }
}
</script>
