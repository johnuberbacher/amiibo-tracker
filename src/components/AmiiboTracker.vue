<template>
  <b-container>
    <b-modal
      header-class="border-0"
      id="modal-1"
      size="lg"
      centered
      content-class="rounded shadow border-0"
      body-class="pb-3 pb-xl-5 pb-md-4 px-md-4 px-xl-5 pt-0"
      hide-footer
    >
      <b-row class="align-items-center">
        <b-col lg="5">
          <div
            class="w-100 mb-4 mb-lg-0"
            style="
              padding-bottom: 100%;
              background-position: center top;
              background-size: contain;
              background-repeat: no-repeat;
            "
            v-bind:style="{
              backgroundImage: 'url(' + amiiboDetails.image + ')',
            }"
          ></div>
        </b-col>
        <b-col lg="7">
          <h1 class="mb-4 font-weight-bold text-center text-lg-left">
            {{ amiiboDetails.name }}
          </h1>
          <div class="px-4 py-2 bg-light rounded border mb-4">
            <h6
              class="font-weight-bold d-flex flex-row justify-content-between my-3"
            >
              <span class="text-muted">Character:</span>
              <span>{{ amiiboDetails.character }}</span>
            </h6>
            <h6
              class="font-weight-bold d-flex flex-row justify-content-between my-3"
            >
              <span class="text-muted">Amiibo Series:</span>
              <span>{{ amiiboDetails.amiiboSeries }}</span>
            </h6>
            <h6
              class="font-weight-bold d-flex flex-row justify-content-between my-3"
            >
              <span class="text-muted">Game Series:</span>
              <span>{{ amiiboDetails.gameSeries }}</span>
            </h6>
            <h6
              class="font-weight-bold d-flex flex-row justify-content-between my-3"
            >
              <span class="text-muted">Release:</span>
              <div class="text-right">
                <span
                  v-for="(release, releaseID) in amiiboDetails.release"
                  v-bind:key="release"
                  class="
                    mb-1
                    ml-1
                    bg-white
                    border
                    rounded
                    d-inline-block
                    py-1
                    px-2
                    font-weight-bold
                    small
                  "
                >
                  <span class="text-uppercase">{{ releaseID }}</span> - {{ release }}
                </span>
              </div>
            </h6>
          </div>
          <div class="px-4 py-2 bg-light rounded border">
            <h6
              class="font-weight-bold mb-3"
            >Available in: 
            </h6>
          </div>
        </b-col>
      </b-row>
    </b-modal>
    <b-row class="align-items-stretch mt-5">
      <b-col
        sm="6"
        md="4"
        class="mt-5 mb-5"
        v-for="amiibo in amiiboList"
        v-bind:key="amiibo.id"
      >
        <div
          role="button"
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
  </b-container>
</template>

<script>
import axios from "axios";
export default {
  name: "Amiibo Tracker",
  props: {
    msg: String,
  },
  data() {
    return {
      amiiboList: [],
      amiiboDetails: [],
    };
  },
  methods: {
    async viewDetails(amiibo) {
      axios
        .get("https://amiiboapi.com/api/amiibo/?name=" + amiibo.name + "")
        .then((response) => {
          this.amiiboDetails = response.data.amiibo[0];
          this.$bvModal.show("modal-1");
        });
    },
    async loadData() {
      axios.get("https://amiiboapi.com/api/amiibo/").then((response) => {
        this.amiiboList = response.data["amiibo"];
      });
    },
    catch(error) {
      console.log(error);
    },
  },
  created() {
    return this.loadData();
  },
};
</script>