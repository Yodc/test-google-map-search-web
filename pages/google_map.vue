<template>
  <v-row dense>
    <v-col cols="11" sm="11">
      <v-text-field v-model="search_text" label="Search Place" outlined clearable @keyup.enter="onEnter" />
    </v-col>
    <v-col cols="1" sm="1">
      <v-btn x-large color="primary" @click="click_search">
        search
      </v-btn>
    </v-col>
    <v-col cols="12" sm="12">
      <v-expansion-panels>
        <v-expansion-panel v-for="(item, index) in items" :key="index">
          <v-expansion-panel-header>
            {{ item.name }}
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            {{ item?.formatted_address }}
            {{ item?.plus_code?.compound_code }}
            <a :href="item?.photos?.[0]?.html_attributions?.[0].slice(9,item?.photos?.[0]?.html_attributions?.[0].indexOf('>')-1)" target="_blank">
              google map link
            </a>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'GoogleMapPage',
  methods: {
    async search_google_map (text) {
      if (this.search_text !== '' && this.search_text.toLocaleLowerCase() !== 'bang sue') {
        text = this.search_text
      }

      const { data } = await this.$axios.get('http://localhost:8000/api/google_map', { params: { search_text: text } })
      this.items = data?.results
    },
    click_search () {
      this.search_google_map()
    },
    onEnter () {
      this.search_google_map()
    }
  },
  async asyncData ({ $axios }) {
    const { data } = await $axios.get('http://localhost:8000/api/google_map')
    return { items: data?.results }
  },
  data () {
    return {
      items: []
    }
  }
}
</script>
