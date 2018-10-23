<template>
  <AdminTemplate :title='title' :btnText="btnText" @create="credentialsCreate">
    <div slot="content">
      <div class="text-xs-center mt-5" v-if="!credentials.data">
        <v-progress-circular
        indeterminate
        color="purple"
        ></v-progress-circular>
      </div>
      <v-data-table
          :headers="headers"
          :items="credentials.data"
          hide-actions
          class="elevation-1"
        >
          <template slot="items" slot-scope="props">
            <td>{{ props.item.name }}</td>
            <td class="text-xs-left">--</td>
            <td class="text-xs-left">{{ props.item.createdAt | datefmt('YYYY-MM-DD HH:mm:ss') }}</td>
            <td class="text-xs-left text--primary key">
              <v-list-tile-content style="width:50px">
                <v-list-tile-title>
                  {{props.item.publicKey}}
                  {{props.item.privateKey}}
                </v-list-tile-title>
              </v-list-tile-content>
              <v-btn color="info"  small @click="look(props.item.publicKey, props.item.privateKey)">查看</v-btn>
            </td>
          </template>
        </v-data-table>
        <v-dialog v-model="lookKey" width="600px">
          <v-card>
            <v-card-title>
              <span class="headline">公钥/私钥</span>
            </v-card-title>
            <v-card-text>
              <h3>公钥</h3>
              <p class="a">{{key.publicKey}}</p>
              <br>
              <h3>私钥</h3>
              <p class="a">{{key.privateKey}}</p>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="green darken-1" flat="flat" @click="lookKey = false">关闭</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
    </div>
  </AdminTemplate>
</template>

<script>
  import { createCredentials, getCredentials } from '@/api/axios/api'
  import AdminTemplate from '@/components/Admin/SystemManagement/AdminTemplate'
  export default {
    data () {
      return {
        lookKey: false,
        title: 'RSA证书',
        btnText: '添加 Credential',
        key: {
          publicKey: '',
          privateKey: ''
        },
        credentials: {
          data: []
        },
        headers: [
          {
            text: '名称',
            align: 'left',
            sortable: false
          },
          { text: '创建人', sortable: false },
          { text: '创建时间', sortable: false },
          { text: 'Fingerprint', sortable: false }
        ]
      }
    },
    created () {
      this.credentialsGet()
    },
    methods: {
      credentialsCreate (name) {
        createCredentials(name).then(res => {
          if (res.data) {
            this.credentialsGet()
          }
        }).catch(err => {
          console.log(err)
        })
      },
      credentialsGet () {
        getCredentials().then(res => {
          this.credentials.data = res.data
        })
      },
      look (publicKey, privateKey) {
        this.key.publicKey = publicKey
        this.key.privateKey = privateKey
        this.lookKey = true
      }
    },
    components: {
      AdminTemplate
    }
  }
</script>

<style>
.key {
  display: flex;
  align-items: center;
  justify-content: center;
}
.a {
  word-wrap : break-word;
}
</style>
