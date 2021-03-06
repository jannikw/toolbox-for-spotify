<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <b-alert
          class="mt-3"
          variant="danger"
          dismissible
          :show="error !== undefined"
          @dismissed="resetError()"
        >Unexpected error: {{ error }}</b-alert>
      </div>
    </div>
    <template v-if="display == 'settings'">
      <query-settings/>
      <div class="row">
        <div class="col">
          <ActionButton
            class="float-right mt-3"
            variant="success"
            iconSide="right"
            icon="fa-arrow-alt-circle-right"
            @click="executeQuery()"
            :canExecute="canExecuteQuery"
            :executing="executing == 'query'"
          >Show Results</ActionButton>
        </div>
      </div>
    </template>
    <template v-if="display == 'results'">
      <query-results/>
      <div class="row">
        <div class="col">
          <b-button class="mt-3" variant="secondary" @click="display = 'settings'">
            <i class="fas fa-arrow-alt-circle-left mr-2"></i> Edit Query
          </b-button>
          <ActionButton
            class="float-right mt-3"
            variant="success"
            iconSide="right"
            icon="fa-arrow-alt-circle-right"
            @click="executeSelectResults()"
            :canExecute="canSaveResults"
            :executing="executing == 'select'"
          >Save Results</ActionButton>
        </div>
      </div>
    </template>
    <template v-if="display == 'save'">
      <song-destination/>
      <div class="row">
        <div class="col">
          <b-button class="mt-3" variant="secondary" @click="display = 'results'">
            <i class="fas fa-arrow-alt-circle-left mr-2"></i> Edit Results
          </b-button>
        </div>
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import QuerySettings from "@/components/QuerySettings.vue";
import QueryResults from "@/components/QueryResults.vue";
import SongDestination from "@/components/SongDestination.vue";
import ActionButton from "@/components/shared/ActionButton.vue";

import * as query from "../toolbox/query";

import { createHelpers } from "vuex-map-fields";
import { mapGetters, mapActions } from "vuex";

const { mapFields, mapMultiRowFields } = createHelpers({
  getterType: "query/getField",
  mutationType: "query/updateField"
});

export default Vue.extend({
  name: "query",
  components: { QuerySettings, QueryResults, SongDestination, ActionButton },
  data() {
    return {};
  },
  computed: {
    ...mapFields(["display", "executing", "error"]),
    ...mapGetters("query", ["canExecuteQuery", "canSaveResults"]),
  },
  methods: {
    ...mapActions("query", [
      "executeQuery",
      "executeSelectResults",
      "resetError"
    ])
  }
});
</script>

<style>
</style>
