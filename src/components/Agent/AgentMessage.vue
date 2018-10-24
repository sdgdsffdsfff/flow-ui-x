<template>
    <v-card height="300px">
        <v-card-title style="justify-content:center;"  v-if="!agents">没有检测到Agent</v-card-title>
        <v-data-table
            v-if="agents"
            :headers="headers"
            :items="agents"
            hide-actions
            class="elevation-1"
        >
            <template slot="items" slot-scope="props">
                <td class="text-xs-left">{{props.item.name}}</td>
                <td class="text-xs-left">--</td>
                <td class="text-xs-left">{{props.item.status}}</td>
            </template>
        </v-data-table>
    </v-card>
</template>

<script>
  import { getAgents } from '@/api/axios/api'
  export default {
    data () {
      return {
        agents: [],
        headers: [
          { text: 'name', sortable: false },
          { text: 'Job', sortable: false },
          { text: 'Status', sortable: false }
        ]
      }
    },
    created () {
      getAgents().then(res => {
        this.agents = res.data.data
      })
    }
  }
</script>

<style>
</style>
