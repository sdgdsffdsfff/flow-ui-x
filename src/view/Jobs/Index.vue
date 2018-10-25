<template>
    <v-card height='100%' width="100%">
      <!-- 运行工作流错误状态 -->
      <v-alert
        :value="alert"
        type="error"
        transition="scale-transition"
        icon="warning"
        class="alert"
      >
        Please configure yml.
      </v-alert>
        <v-card-title>
          <h2 class="pr-4">
            <v-icon>layers</v-icon>
            {{this.$route.params.id}}
          </h2>
          <v-chip label color="" outline text-color="black" @click="yml">
              <v-icon left>settings</v-icon>工作流设置
          </v-chip>
        </v-card-title>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            :loading="loading"
            :disabled="loading"
            color="success"
            @click.native="jobrun"
          >
            运行工作流
          </v-btn>
        </v-card-actions>
        <v-card-text>
          <JobItem></JobItem>
        </v-card-text>
    </v-card>
</template>

<script>
  import { jobRun } from '@/api/axios/api'
  import JobItem from '@/components/Jobs/JobItem'
  export default {
    name: 'Jobs',
    data () {
      return {
        loading: false,
        alert: false
      }
    },
    methods: {
      // 工作流设置
      yml () {
        this.$router.push({path: `/flows/${this.$route.params.id}/yml`})
      },
      // 运行工作流
      jobrun () {
        jobRun(this.$route.params.id).then(res => {
          this.loading = true
          if (res.data.code === 200) {
            this.loading = false
          } else if (res.data.code === 404) {
            this.loading = false
            this.alert = true
            setTimeout(() => {
              this.alert = false
            }, 1000)
          }
        }).catch(err => {
          console.log(err)
        })
      }
    },
    components: {
      JobItem
    }
  }
</script>

<style scoped>
.alert {
  position: absolute;
  width: 100%;
  z-index: 1000;
}

</style>
