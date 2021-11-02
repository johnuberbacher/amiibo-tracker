<template>
  <b-container>
    <div
      v-if="loading"
      class="vh-100 d-flex align-items-center justify-content-center"
    >
      <div class="spinner-border text-primary" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>
    <b-row class="align-items-stretch mt-5">
      <b-col sm="12" md="12">
        <div class="shadow rounded mb-5 bg-white position-relative">
       
            <b-icon class="position-absolute text-primary"
              icon="search"
              style="width: 18px; height: 18px; right: 1rem; top: 1rem;"
            ></b-icon>
            <b-form-input
              v-model="filterInput"
              placeholder="Search Amiibo..."
              class="p-4 rounded"
            ></b-form-input>
        </div>
      </b-col>
      <b-col
        sm="6"
        md="4"
        class="mt-5 mb-5"
        v-for="(amiibo, index) in filteredAmiiboList"
        v-bind:key="index"
      >
        <div
          role="button"
          v-bind:class="{ collected: amiiboIsCollected(amiibo) }"
          v-bind:title="amiiboIsCollected(amiibo)"
          @click="viewDetails(amiibo)"
          class="
            card
            d-flex
            flex-column
            justify-content-center
            pb-4
            px-4 px-xl-5
            pb-xl-5
            text-center
            shadow
            h-100
          "
        >
          <div
            class="w-100 mt-n5 mb-4 mb-xl-5"
            style="
              padding-bottom: 75%;
              background-position: center top;
              background-size: contain;
              background-repeat: no-repeat;
            "
            v-bind:style="{ backgroundImage: 'url(' + amiibo.image + ')' }"
          ></div>
          <div class="w-100">
            <h5 class="font-weight-bold">{{ amiibo.name }}</h5>
            <h6 class="text-muted">
              {{ amiibo.gameSeries }} - {{ amiibo.amiiboSeries }}
            </h6>
          </div>
        </div>
      </b-col>
    </b-row>
    <Details
      v-on:markCollected="markCollected(collectedAmiibo)"
      :amiiboData="this.amiiboDetails"
      :isCollected="this.collected.includes(this.amiiboDetails[0].tail)"
    ></Details>
  </b-container>
</template>

<script>
import axios from "axios";
import Details from "./Details.vue";
export default {
  name: "Amiibo Tracker",
  props: {
    msg: String,
  },
  components: {
    Details,
  },
  data() {
    return {
      loading: true,
      amiiboList: [],
      amiiboDetails: {},
      collected: [],
      collectedAmiibo: "",
      filterInput: "",
    };
  },
  methods: {
    async loadData() {
      axios.get("https://amiiboapi.com/api/amiibo/").then((response) => {
        this.amiiboList = response.data["amiibo"];
        this.loading = false;
        axios
          .get("https://amiiboapi.com/api/amiibo/?tail=00000002")
          .then((response) => {
            this.amiiboDetails = response.data.amiibo;
          });
      });
    },
    async viewDetails(amiibo) {
      axios
        .get(
          "https://amiiboapi.com/api/amiibo/?tail=" + amiibo.tail + "&showusage"
        )
        .then((response) => {
          this.amiiboDetails = response.data.amiibo;
          this.$bvModal.show("modal-1");
        });
    },
    amiiboIsCollected(amiibo) {
      return this.collected.includes(amiibo.tail);
    },
    markCollected() {
      if (this.collected.includes(this.amiiboDetails[0].tail)) {
        this.collected = this.collected.filter(
          (v) => v !== this.amiiboDetails[0].tail
        );
      } else {
        this.collected.push(this.amiiboDetails[0].tail);
      }
      this.$bvModal.hide("modal-1");
    },
    catch(error) {
      console.log(error);
    },
  },
  mounted() {
    return this.loadData();
  },
  computed: {
    filteredAmiiboList() {
      console.log("amiiboList:");
      console.log(this.amiiboList[0]);
      // function to compare names
      function compare(a, b) {
        if (a.name < b.name) return -1;
        if (a.name > b.name) return 1;
        return 0;
      }
      return this.amiiboList
        .filter((amiibo) => {
          return amiibo.name
            .toLowerCase()
            .includes(this.filterInput.toLowerCase());
        })
        .sort(compare);
    },
  },
};
</script>