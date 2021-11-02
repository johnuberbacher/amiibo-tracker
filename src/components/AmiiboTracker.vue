<template>
  <b-container>
    <div
      role="button"
      class="
        position-fixed
        bg-white
        rounded-circle
        shadow
        d-flex
        align-items-center
        justify-content-center
      "
      style="top: 1rem; right: 1rem; width: 50px; height: 50px; z-index: 3"
    >
      <b-icon
        icon="menu-button-wide"
        style="width: 25px; height: 25px"
      ></b-icon>
    </div>
    <div
      v-if="loading"
      class="vh-100 d-flex align-items-center justify-content-center"
    >
      <div class="spinner-border text-primary" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>
    <b-row class="align-items-stretch mt-5">
      <b-col
        sm="6"
        md="4"
        class="mt-5 mb-5"
        v-for="(amiibo, index) in amiiboList"
        v-bind:key="index"
      >
        <div
          role="button"
          v-bind:class="{ collected: amiiboIsCollected(amiibo) }"
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
      collectedAmiibo: '',
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
        .get("https://amiiboapi.com/api/amiibo/?tail=" + amiibo.tail + "")
        .then((response) => {
          this.amiiboDetails = response.data.amiibo;
          this.$bvModal.show("modal-1");
        });
    },
    amiiboIsCollected(amiibo) {
      return this.collected.includes(amiibo.tail);
    },
    markCollected() {
      this.collected.push(this.amiiboDetails[0].tail);
      this.$bvModal.hide("modal-1");
    },
    catch(error) {
      console.log(error);
    },
  },
  mounted() {
    return this.loadData();
  },
};
</script>