<template>
  <div>
    <v-data-table
        :items="jobs"
        class="elevation-1"
        hide-actions
        :headers="headers"
        :loading="loading"
    >
    <template slot="items" slot-scope="props">
        <td @click="jobdetail(props.item)">
        <v-list>
            <v-list-tile>
            <v-list-tile-content>
                <v-list-tile-title v-text="props.item.status"></v-list-tile-title>
            </v-list-tile-content>
            <v-list-tile-content class="branch">
                <v-list-tile-title>
                <h4>#{{ props.item.buildNumber }} {{ props.item.context.FLOWCI_GIT_BRANCH }}</h4>
                </v-list-tile-title>
            </v-list-tile-content>
            <v-list-tile-content>
                <v-list-tile-sub-title>提交 ID -</v-list-tile-sub-title>
            </v-list-tile-content>
            <v-list-tile-content>
                <v-list-tile-sub-title inset>变更对比 -</v-list-tile-sub-title>
                <v-list-tile-sub-title inset>构建于 -</v-list-tile-sub-title>
            </v-list-tile-content>
            </v-list-tile>
        </v-list>
        </td>
    </template>
    </v-data-table>
    <div class="text-xs-center pt-2">
        <v-pagination v-model="page" :length="pages"></v-pagination>
    </div>
  </div>
</template>

<script>
  import { jobsList } from '@/api/axios/api'
  import { mapState } from 'vuex'
  export default {
    data () {
      return {
        page: 1,
        headers: [
          { text: 'list', sortable: false }
        ],
        loading: true,
        size: 10,
        pages: 0,
        jobs: []
      }
    },
    created () {
      this.getJobList(0)
    },
    methods: {
      jobdetail (val) {
        this.$router.push({path: `/flows/${this.$route.params.id}/jobs/${val.buildNumber}`})
      },
      getJobList (page) {
        jobsList(this.$route.params.id, this.size, page).then(res => {
          this.pages = res.data.data.totalPages
          this.jobs = res.data.data.content
          res.data.data.content ? this.loading = false : this.loading = true
        }).catch(err => {
          console.log(err)
        })
      }
    },
    computed: {
      ...mapState({
        jobsStatus: state => state.jobs.JobsStatus
      })
    },
    watch: {
      //  分页改变
      page (val) {
        this.getJobList(val - 1)
      },
      //  每次监听到路由变换的时候 渲染不同的JOBS
      $route (to, form) {
        this.getJobList(0)
      },
      //  推送状态监听
      jobsStatus (val) {
        if (val.event === 'NEW_CREATED') {
          this.jobs.unshift(val.body)
        } else if (val.event === 'STATUS_CHANGE') {
          this.jobs.find((data, i) => {
            data.buildNumber === val.body.buildNumber && this.$set(this.jobs, i, val.body)
          })
        }
      }
    }
  }
</script>

<style scoped>
.v-list {
  cursor: pointer;
  background: transparent;
}
.v-list__tile__sub-title {
  font-size: 12px;
}
.branch {
  border-right: 1px solid rgb(212, 220, 227);
  margin-right: 20px;
  width: 60%;
  box-sizing: border-box;
}
</style>
