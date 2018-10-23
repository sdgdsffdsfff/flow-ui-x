<template>
  <AdminTemplate :title="title" :btnText="btnText" @create="agentCreate">
  </AdminTemplate>
</template>

<script>
  import AdminTemplate from '@/components/Admin/SystemManagement/AdminTemplate'
  export default {
    data () {
      return {
        title: 'Agent',
        btnText: '添加 Agent'
      }
    },
    methods: {
      agentCreate (name) {
        console.log(name)
      }
    },
    components: {
      AdminTemplate
    }
  }
</script>

<style>

</style>
