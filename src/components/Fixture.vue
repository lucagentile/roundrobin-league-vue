<template>
  <div class="fixture panel">
      <div class="panel-header text-center">
        <span class="panel-title">Day {{ index+1 }}</span>
        <button class="btn btn-link btn-action delete-btn btn-sm" v-on:click="deleteFixture"><i class="icon icon-cross"></i></button>
      </div>
      <div class="body">
        <table v-if="!noMatches" class="table table-striped table-hover text-center">
          <thead>
              <th></th>
              <th>Home</th>
              <th>Score</th>
              <th>Away</th>
          </thead>
          <tbody>
            <match :key="match.id" v-for="match in fixture.matches" v-bind:matchProp="match" @deleteMe="deleteMatch"></match>
          </tbody>
        </table>
      </div><!-- .body -->
    <div class="panel-footer">
      <button class="btn btn-primary btn-block" @click="createMatch(fixture)">Add match</button>
    </div>
  </div>
</template>

<script>
  import draggable from 'vuedraggable'
  import match from './Match.vue'
  import { mapActions } from 'vuex'

  export default {
    props: {
      fixtureProp: {
        type: Object,
        required: true
      },
      roundId: {
        type: Number,
        required: true
      },
      teams: {
        type: Array,
        required: true
      },
      index: {
        type: Number,
        required: true
      }
    },
    data () {
      return {
        fixture: this.fixtureProp
      }
    },
    computed: {
      noMatches: {
        get () {
          return this.fixture.matches.length === 0
        }
      }
    },
    methods: {
      ...mapActions([
        'createMatch'
      ]),
      deleteFixture () {
        this.$store.dispatch('deleteFixture', {roundId: this.roundId, fixture: this.fixture})
      },
      deleteMatch (match) {
        let matches = this.fixture.matches
        let index = matches.indexOf(matches.find((x) => x.id === match.id))
        matches.splice(index, 1)
        this.fixture.matches = matches
      }
    },
    components: {
      draggable, match
    }
  }
</script>

<style scoped>
</style>
