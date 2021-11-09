<template>
  <b-modal
    header-class="border-0"
    id="modal-1"
    size="xl"
    centered
    modal-class="pl-0"
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
            backgroundImage: 'url(' + amiiboData[0].image + ')',
          }"
        ></div>
      </b-col>
      <b-col lg="7">
        <h1 class="mb-4 font-weight-bold text-center text-lg-left">
          {{ amiiboData[0].name }}
        </h1>
        <div class="px-4 py-2 bg-light rounded border mb-4">
          <h6
            class="
              font-weight-bold
              d-flex
              flex-row
              justify-content-between
              my-3
            "
          >
            <span class="text-muted mr-4">Character:</span>
            <span>{{ amiiboData[0].character }}</span>
          </h6>
          <h6
            class="
              font-weight-bold
              d-flex
              flex-row
              justify-content-between
              my-3
            "
          >
            <span class="text-muted mr-4">Amiibo Series:</span>
            <span>{{ amiiboData[0].amiiboSeries }}</span>
          </h6>
          <h6
            class="
              font-weight-bold
              d-flex
              flex-row
              justify-content-between
              my-3
            "
          >
            <span class="text-muted mr-4">Game Series:</span>
            <span>{{ amiiboData[0].gameSeries }}</span>
          </h6>
          <h6
            class="
              font-weight-bold
              d-flex
              flex-row
              justify-content-between
              my-3
            "
          >
            <span class="text-muted mr-4">Release:</span>
            <div class="text-right">
              <span
                v-for="(release, releaseID) in amiiboData[0].release"
                v-bind:key="releaseID"
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
                <span class="text-uppercase text-primary">{{ releaseID }}</span
                >:
                {{ release }}
              </span>
            </div>
          </h6>
        </div>
        <div class="p-4 bg-light rounded border">
          <h6 class="font-weight-bold text-muted mb-3">
            Compatible Nintendo Switch Games:
          </h6>
          <div
            v-for="(game, gameID) in amiiboData[0].gamesSwitch"
            v-bind:key="gameID"
            v-b-tooltip.hover
            :title="game.amiiboUsage[0].Usage"
            class="
              mb-1
              mr-1
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
            <span class="">{{ game.gameName }}</span>
          </div>
        </div>
      </b-col>
    </b-row>
    <button
      class="btn btn-primary btn-block font-weight-bold mt-4 mt-xl-4"
      @click="handleCollected(amiiboData[0].tail)"
    >
      {{ handleCollectedBtn() }}
    </button>
  </b-modal>
</template>

<script>
export default {
  name: "Details",
  methods: {
    handleCollected: function () {
      this.$emit("markCollected", this.amiiboData[0]);
    },
    handleCollectedBtn: function () {
      if (this.isCollected == true) {
        return "Mark as Uncollected";
      } else {
        return "Mark as Collected";
      }
    },
  },
  props: {
    amiiboData: [],
    isCollected: {
      type: Boolean,
    },
  },
};
</script>