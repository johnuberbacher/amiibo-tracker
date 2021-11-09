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
    <b-row class="align-items-stretch justify-content-center mt-5">
      <b-col sm="12" md="12">
        <div class="d-flex flex-row align-items-center mb-5">
          <div
            class="shadow bg-white position-relative w-100"
            style="border-radius: 100rem"
          >
            <b-icon
              class="position-absolute text-primary"
              icon="search"
              style="width: 18px; height: 18px; left: 1.5rem; top: 1.25rem"
            ></b-icon>
            <b-icon
              v-if="!this.filterInput == ''"
              @click="resetInput"
              role="button"
              class="position-absolute text-muted"
              icon="x-circle"
              style="width: 18px; height: 18px; right: 1.5rem; top: 1.25rem"
            ></b-icon>
            <b-form-input
              style="height: 60px; border-radius: 100rem; padding-left: 4rem"
              v-model="filterInput"
              placeholder="Search Amiibo..."
              class="py-4"
            ></b-form-input>
          </div>
          <b-dropdown
            toggle-class="
            text-decoration-none 
            border-0 
            rounded-circle
              d-flex
              align-items-center
              justify-content-center
              bg-primary"
            no-caret
            right
            menu-class="shadow mt-3 bg-white"
            id="dropdown-1"
            style="min-width: 55px; width: 55px; height: 55px"
            class="rounded-circle shadow ml-3"
          >
            <template #button-content>
              <b-icon
                icon="sort-down"
                style="width: 25px; height: 25px"
              ></b-icon
              ><span class="sr-only">Search</span>
            </template>
            <b-dropdown-item @click="clearAmiiboSeries()">
              Show All
            </b-dropdown-item>
            <b-dropdown-item
              v-for="(game, index) in amiiboSeries"
              v-bind:key="index"
              @click="sortAmiiboSeries(game.name)"
              >{{ game.name }}</b-dropdown-item
            >
          </b-dropdown>
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
          v-if="amiiboIsCollected(amiibo)"
          class="
            ribbon
            font-weight-bold
            py-3
            px-5
            text-uppercase
            position-absolute
            bg-primary
          "
        >
          Collected!
        </div>
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
              background-position: center center;
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
    <div class="w-100 text-center mb-5">
      <a
        href="https://github.com/johnuberbacher/amiibo-tracker"
        target="_blank"
        rel="noopener noreferrer"
        class="text-muted small"
        ><u>John Uberbacher - View on GitHub</u></a
      >
    </div>
    <b-icon
      role="button"
      v-if="scY > 300"
      @click="scrollUp"
      class="position-fixed mb-1 rounded-circle p-0 text-dark"
      icon="arrow-up-circle-fill"
      style="
        width: 40px;
        height: 40px;
        right: 0.5rem;
        bottom: 0.5rem;
        opacity: 0.75;
      "
    ></b-icon>
  </b-container>
</template>

<script>
import axios from "axios";
import Details from "./Details.vue";
export default {
  name: "Amiibo Tracker",
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
      amiiboSeries: [],
      sortedAmiiboSeries: "",
      filterInput: "",
      scTimer: 0,
      scY: 0,
    };
  },
  methods: {
    async loadData() {
      axios
        .get(
          "https://amiiboapi.com/api/amiibo/?type=Figure" +
            this.sortedAmiiboSeries
        )
        .then((response) => {
          this.amiiboList = response.data["amiibo"];
          this.loading = false;
          axios
            .get("https://amiiboapi.com/api/amiibo/?tail=00000002")
            .then((response) => {
              this.amiiboDetails = response.data.amiibo;
            });
        });
    },
    async loadAmiiboSeries() {
      axios.get("https://amiiboapi.com/api/amiiboseries/").then((response) => {
        this.amiiboSeries = response.data.amiibo;
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
    handleScroll: function () {
      if (this.scTimer) return;
      this.scTimer = setTimeout(() => {
        this.scY = window.scrollY;
        clearTimeout(this.scTimer);
        this.scTimer = 0;
      }, 100);
    },
    scrollUp: function () {
      window.scrollTo({
        top: 0,
        behavior: "smooth",
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
    resetInput() {
      this.filterInput = "";
    },
    catch(error) {
      console.log(error);
    },
    clearAmiiboSeries() {
      return (this.sortedAmiiboSeries = ""), this.loadData();
    },
    sortAmiiboSeries(amiiboSeries) {
      return (
        (this.sortedAmiiboSeries = "&amiiboSeries=" + amiiboSeries),
        this.loadData()
      );
    },
  },
  mounted() {
    if (localStorage.collected) {
      this.collected = localStorage.collected.split(",");
    }
    window.addEventListener("scroll", this.handleScroll);
    return this.loadData(), this.loadAmiiboSeries();
  },
  computed: {
    filteredAmiiboList() {
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
  watch: {
    collected(collectedAmiibo) {
      localStorage.collected = collectedAmiibo;
    },
  },
};
</script>