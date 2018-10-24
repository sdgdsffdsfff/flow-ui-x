<template>
    <v-flex xs12>
        <v-combobox
            v-model="select"
            :items="items"
            label="tag"
            multiple
            chips
        >
            <template
            slot="selection"
            slot-scope="data"
            >
            <v-chip
                :selected="data.selected"
                :disabled="data.disabled"
                :key="JSON.stringify(data.item)"
                class="v-chip--select-multi"
                @input="data.parent.selectItem(data.item)"
            >
                <v-avatar
                class="accent white--text"
                v-text="data.item.slice(0, 1).toUpperCase()"
                ></v-avatar>
                {{ data.item }}
            </v-chip>
            </template>
        </v-combobox>
    </v-flex>
</template>

<script>
  export default {
    data () {
      return {
        select: [],
        items: []
      }
    },
    watch: {
      select (val) {
        this.$emit('agentTag', val)
      }
    }
  }
</script>

<style>
</style>
