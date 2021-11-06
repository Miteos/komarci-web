<template>
  <div v-if="!dataLoading">
    <v-card class="mt-5">
      <v-card-title>Tablica podataka</v-card-title>
      <v-data-table
        dense
        :items="newData[0]"
        :items-per-page="itemsNumber"
        :headers="headers"
        :expanded.sync="expanded"
        show-expand
        item-key="username"
      >
        <template v-slot:item.school="{ item }">
          {{ getSchool(item.username) }}
        </template>
        <template v-slot:item.data.media="{ item }">
          <a
            :href="
              'http://komarci.biologija.unios.hr:5000/api/media/' +
              item.data.media
            "
            target="_blank"
            >Slika</a
          >
        </template>
        <template v-slot:expanded-item="{ headers, item }">
          <td :colspan="headers.length">
            <div style="min-height: 300px; width: 100%">
              <iframe
                :src="`https://maps.google.com/maps?q=${item.data.latitude},${item.data.longitude}&z=18&amp;output=embed`"
                class="google-map"
                frameborder="0"
                allowfullscreen="true"
                aria-hidden="false"
                tabindex="0"
              ></iframe>
            </div>
          </td>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      itemsNumber: 400,
      expanded: [],
      dataLoading: false,
      headers: [
        {
          text: 'Škola',
          align: 'start',
          value: 'school',
        },
        { text: 'Korisničko ime', value: 'username' },
        { text: 'Inicijali', value: 'data.initials' },
        { text: 'Datum postavljanja klopke', value: 'data.initial_date' },
        { text: 'Geografska dužina', value: 'data.longitude' },
        { text: 'Geografska širina', value: 'data.latitude' },
        { text: '1.tjedan', value: 'data.eggCount_1' },
        { text: '2.tjedan', value: 'data.eggCount_2' },
        { text: '3.tjedan', value: 'data.eggCount_3' },
        { text: '4.tjedan', value: 'data.eggCount_4' },
        { text: 'Slika', value: 'data.media' },
        { text: 'Lokacija', value: 'data-table-expand' },
      ],
      newData: [],
    }
  },
  async asyncData({ app }) {
    const users = await app.$axios.get(`user/all`)
    return {
      data: users.data,
    }
  },
  async mounted() {
    try {
      this.dataLoading = true
      let filteredData = await this.data.filter((a) => a.data != null)
      this.newData.push(filteredData)
    } catch (e) {
      error(e)
    } finally {
      this.dataLoading = false
    }
  },
  methods: {
    getSchool(value) {
      let newValue = value.substring(0, value.indexOf('_'))
      if (newValue === 'osjbi') return 'OŠ Julija Benašića Ilok'
      if (newValue === 'osjlo') return 'OŠ Josipa Lovretića Otok'
      if (newValue === 'osigkvk') return 'OŠ IGK Vinkovci'
      if (newValue === 'osntvk') return 'OŠ Nikole Tesle Vinkovci'
      if (newValue === 'osmv') return 'OŠ Mitnica Vukovar'
      if (newValue === 'ossgv') return 'OŠ Siniše Glavaševića Vukovar'
      if (newValue === 'osikz') return 'OŠ Ivana Kozarca Županja'
      if (newValue === 'osml') return 'OŠ Mate Lovraka'
      if (newValue === 'oszbg') return 'OŠ Zrinskih Babina Greda'
      if (newValue === 'osb') return 'OŠ Bršadin'
      if (newValue === 'osl') return 'OŠ Lovas'
      if (newValue === 'osv') return 'OŠ Vođinci'
      if (newValue === 'osagm') return 'OŠ A.G. Matoš'
      if (newValue === 'osmg') return 'OŠ Matija Gubec'
      if (newValue === 'osfbtl') return 'OŠ fra B. T. Leakovića'
      if (newValue === 'oslip') return 'OŠ Lipovac'
      if (newValue === 'osdt') return 'OŠ Davorin Trstenjak'
      if (newValue === 'ssgmar') return 'Gimnazija M.A.R VInkovci'
      if (newValue === 'sszvvk') return 'Zdrav. i  vet. škola Vinkovci'
      if (newValue === 'osvn') return 'OŠ Vladimir Nazor'
      if (newValue === 'osern') return 'OŠ Ernestinovo'
      if (newValue === 'osretos') return 'OŠ Retfala Osijek'
      if (newValue === 'osamos') return 'OŠ Antuna Mihanovića Osijek'
      if (newValue === 'osjtos') return 'OŠ Jagode Truhelke Osijek'
      if (newValue === 'osdcos') return 'OŠ Dobriše Cesarića Osijek'
      if (newValue === 'ossj') return 'OŠ Stari Jankovci'
      if (newValue === 'osdar') return 'OŠ Darda'
    },
  },
}
</script>

<style>
.google-map {
  display: block;
  width: 100%;
  height: 400px;
  padding: 20px 0;
}
.v-data-table__expanded.v-data-table__expanded__content {
  box-shadow: none !important;
}
</style>
