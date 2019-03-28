<template>
  <div>
    <div class="page-header mt-3 ml-1">
      <h1>Save</h1>
    </div>
    <b-row class="mt-3">
      <b-col>
        <b-card title="Library" sub-title="Save the selected songs to your library.">
          <ActionButton
            class="float-right"
            variant="success"
            iconSide="right"
            icon="fa-arrow-alt-circle-right"
            @click="executeSaveToLibrary()"
            :canExecute="canSaveToLibrary"
            :executing="executing == 'library'"
          >Save</ActionButton>
        </b-card>
      </b-col>
    </b-row>
    <b-row class="mt-3">
      <b-col>
        <b-card title="New Playlist" sub-title="Save the selected songs to a new playlist.">
          <b-form-input type="text" v-model="name" placeholder="Playlist Name"/>
          <div class="row">
            <div class="col-6">
              <b-form-checkbox v-model="publicPlaylist" class="mt-2 ml-2">Public</b-form-checkbox>
            </div>
            <div class="col-6">
              <ActionButton
                class="float-right mt-2"
                variant="success"
                iconSide="right"
                icon="fa-arrow-alt-circle-right"
                @click="executeSaveToNewPlaylist()"
                :canExecute="canSaveToNewPlaylist"
                :executing="executing == 'newPlaylist'"
              >Save</ActionButton>
            </div>
          </div>
        </b-card>
      </b-col>
    </b-row>
    <b-row class="mt-3">
      <b-col>
        <b-card
          title="Existing Playlist"
          sub-title="Save the selected songs to a existing playlist."
        >
          <b-form-select v-model="selectedId" :options="playlistOptions"/>
          <div class="row align-items-center no-gutters">
            <div class="col-4">
              <ActionButton
                class="mt-2"
                variant="info"
                icon="fa-sync"
                iconExecuting="fa-sync"
                @click="executeSelectResults()"
                :executing="executing == 'select'"
              >Refresh Playlists</ActionButton>
            </div>
            <div class="col-4 text-center">
              <b-form-group class="mt-2">
                <b-form-radio-group v-model="mode">
                  <b-form-radio value="append">Append</b-form-radio>
                  <b-form-radio value="replace">Replace</b-form-radio>
                </b-form-radio-group>
              </b-form-group>
            </div>
            <div class="col-4">
              <ActionButton
                class="float-right mt-2"
                variant="success"
                iconSide="right"
                icon="fa-arrow-alt-circle-right"
                @click="executeSaveToExistingPlaylist()"
                :canExecute="canSaveToExistingPlaylist"
                :executing="executing == 'existingPlaylist'"
              >Save</ActionButton>
            </div>
          </div>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import MultiInputLines from "@/components/MultiInputLines.vue";
import ActionButton from "@/components/shared/ActionButton.vue";

import { createHelpers } from "vuex-map-fields";
import { mapActions, mapGetters } from "vuex";

const { mapFields, mapMultiRowFields } = createHelpers({
  getterType: "query/getField",
  mutationType: "query/updateField"
});

export default Vue.extend({
  name: "song-destination",
  components: { MultiInputLines, ActionButton },
  data() {
    return {};
  },
  computed: {
    ...mapFields([
      "executing",
      "sink.new.name",
      "sink.new.publicPlaylist",
      "sink.existing.mode",
      "sink.existing.availablePlaylist",
      "sink.existing.selectedId"
    ]),
    ...mapGetters("query", [
      "canSaveToLibrary",
      "canSaveToNewPlaylist",
      "canSaveToExistingPlaylist"
    ]),
    playlistOptions() {
      return (this as any).availablePlaylist.map((p: any) => ({
        value: p.id,
        text: p.name
      }));
    },
  },
  methods: {
    ...mapActions("query", [
      "executeSelectResults",
      "executeSaveToLibrary",
      "executeSaveToNewPlaylist",
      "executeSaveToExistingPlaylist"
    ])
  }
});
</script>

<style>
</style>
