<template>
    <v-card class="ml-3" width="100%">
        <v-card-title class="blue lighten-4">
            <h3>{{title}}</h3>
            <v-spacer/>
            <v-dialog
                v-model="dialog"
                width="500"
                slot="dialog"
            >
                <v-btn
                    slot="activator"
                    color="green lighten-2"
                    dark
                >
                    {{btnText}}
                </v-btn>

                <v-card>
                    <v-card-title
                    class="headline grey lighten-2"
                    primary-title
                    >
                    名称
                    </v-card-title>

                    <v-card-text>
                      <name-input init-value="" v-on:input="onNameInput"/>
                      <AgentTag v-if="tagTrigger" @agentTag="agentTag"></AgentTag>
                    </v-card-text>

                    <v-divider></v-divider>

                    <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                        color="primary"
                        flat
                        @click="create()"
                    >
                        生成
                    </v-btn>
                    </v-card-actions>
                </v-card>
            </v-dialog>
        </v-card-title>
        <slot name="content"></slot>
    </v-card>
</template>

<script>
  import NameInput from '@/components/CreateFlow/NameInput'
  import AgentTag from '@/components/Agent/AgentTag'
  export default {
    data () {
      return {
        dialog: false,
        name: '',
        tags: []
      }
    },
    props: {
      title: {
        type: String,
        default: ''
      },
      btnText: {
        type: String,
        default: ''
      },
      tagTrigger: {
        type: Boolean,
        default: false
      }
    },
    methods: {
      create () {
        this.dialog = false
        this.tags ? this.$emit('create', this.name, this.tags) : this.$emit('create', this.name)
      },
      onNameInput (name, errors) {
        this.name = name
      },
      agentTag (tags) {
        this.tags = tags
      }
    },
    components: {
      NameInput,
      AgentTag
    }
  }
</script>

<style>

</style>
