<template>
    <v-layout align-start>
        <v-btn-toggle  v-model="toggle_exclusive">
            <v-btn class="btn" flat to="/admin/systemmanagement/credentials">
                <v-icon left class="material-icons">book</v-icon>
                <span class="ml-3">Credentials</span>
            </v-btn>
            <v-btn class="btn" flat to="/admin/systemmanagement/agent">
                <v-icon left class="material-icons">dvr</v-icon>
                <span class="ml-3">Agent</span>
            </v-btn>
        </v-btn-toggle>
        <router-view></router-view>
    </v-layout>
</template>

<script>
  export default {
    name: 'SystemMangement',
    data () {
      return {
        toggle_exclusive: 0
      }
    }
  }
</script>
<style lang="scss" scoped>
  .v-btn-toggle {
    flex-flow: column;
  }
</style>
<style>
  .btn > .v-btn__content {
    justify-content: flex-start !important;
  }
</style>
