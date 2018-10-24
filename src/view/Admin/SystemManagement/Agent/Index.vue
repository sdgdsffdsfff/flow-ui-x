<template>
  <AdminTemplate :title="title" :tagTrigger=true :btnText="btnText" @create="agentCreate">
    <div slot="content">
      <div class="text-xs-center mt-5" v-if="!agents">
        <v-progress-circular
        indeterminate
        color="purple"
        ></v-progress-circular>
      </div>
      <v-data-table
        :headers="headers"
        :items="agents"
        hide-actions
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <td>{{props.item.status}}</td>
          <td class="text-xs-left">{{props.item.name}}</td>
          <td class="text-xs-left">--</td>
          <td class="text-xs-left">{{props.item.token}}</td>
          <td class="text-xs-left">--</td>
        </template>
      </v-data-table>
    </div>
  </AdminTemplate>
</template>

<script>
  import { createAgent, getAgents } from '@/api/axios/api'
  import AdminTemplate from '@/components/Admin/SystemManagement/AdminTemplate'
  export default {
    data () {
      return {
        title: 'Agent',
        btnText: '添加 Agent',
        headers: [
          {
            text: '运行状态',
            align: 'left',
            sortable: false
          },
          { text: 'Agent', sortable: false },
          { text: '任务', sortable: false },
          { text: 'token', sortable: false },
          { text: '操作', sortable: false }
        ],
        agents: []
      }
    },
    created () {
      this.agentsGet()
    },
    methods: {
      agentCreate (name, tags) {
        createAgent(name, tags).then(res => {
          res.data && this.agentsGet()
        })
      },
      agentsGet () {
        getAgents().then(res => {
          this.agents = res.data.data
        })
      }
    },
    components: {
      AdminTemplate
    }
  }
</script>

<style>

</style>
