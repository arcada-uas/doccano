<template>
  <v-data-table
    :value="value"
    :headers="headers"
    :items="items"
    :search="search"
    :loading="isLoading"
    :loading-text="$t('generic.loading')"
    :no-data-text="$t('vuetify.noDataAvailable')"
    :footer-props="{
      'showFirstLastPage': true,
      'items-per-page-options': [5, 10, 15, $t('generic.all')],
      'items-per-page-text': $t('vuetify.itemsPerPageText'),
      'page-text': $t('dataset.pageText')
    }"
    item-key="id"
    show-select
    @input="$emit('input', $event)"
  >
    <template v-slot:top>
      <v-text-field
        v-model="search"
        prepend-inner-icon="search"
        :label="$t('generic.search')"
        single-line
        hide-details
        filled
      />
    </template>
    <template v-slot:[`item.background_color`]="props">
      <v-chip
        :color="props.item.background_color"
        :text-color="$contrastColor(props.item.background_color)"
      >
        {{ props.item.background_color }}
      </v-chip>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        @click="$emit('edit', item)"
      >
        mdi-pencil
      </v-icon>
    </template>
  </v-data-table>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue'
import { LabelDTO } from '@/services/application/label.service'

export default Vue.extend({
  props: {
    isLoading: {
      type: Boolean,
      default: false,
      required: true
    },
    items: {
      type: Array as PropType<LabelDTO[]>,
      default: () => [],
      required: true
    },
    value: {
      type: Array as PropType<LabelDTO[]>,
      default: () => [],
      required: true
    }
  },

  data() {
    return {
      search: ''
    }
  },

  computed: {
    headers() {
      return [
        { text: this.$t('generic.name'),    value: 'text' },
        { text: this.$t('labels.shortkey'), value: 'suffix_key' },
        { text: this.$t('labels.color'),    value: 'background_color' },
        { text: 'Actions', value: 'actions', sortable: false },
      ]
    }
  }
})
</script>
