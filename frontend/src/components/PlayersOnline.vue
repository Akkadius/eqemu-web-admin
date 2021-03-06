<template>
  <div class="row">
    <div class="col-12">
      <div class="card">
        <div class="card-header">
          <div class="col">

            <form>
              <div class="input-group input-group-flush">
                <div class="input-group-prepend">
                    <span class="input-group-text">
                      <i class="fe fe-search"></i>
                    </span>
                </div>
                <input
                  class="list-search form-control"
                  type="search"
                  placeholder="Search for player(s) or zone..."
                  @keyup="filterPlayers"
                  v-model="playerSearchFilter">
              </div>
            </form>
          </div>

          <div class="col-auto">
            <h4 class="card-header-title" v-if="loaded">Players Online ({{ clientList ? clientList.length : 0 }})</h4>
            <h4 class="card-header-title" v-if="!loaded">Loading...</span></h4>
          </div>

        </div>

        <app-loader :is-loading="!loaded" padding="4"></app-loader>

        <div class="table-responsive" v-if="filteredClientList">

          <div v-if="Object.keys(filteredClientList).length > listLimitSize && !fullList" class="m-4">
            <b-alert variant="primary" show><i class="fe fe-info"></i>
              Too many online to display, for full list see
              <router-link class="ml-2 btn btn-sm btn-white" to="/players-online"><i class="fe fe-user"></i> Players
                Online
              </router-link>
            </b-alert>
          </div>

          <table class="table table-sm table-nowrap players-online">
            <thead>
            <tr>
              <th style="width: 100px"></th>
              <th>Player</th>
              <th style="width: 75px">Level</th>
              <th style="min-width: 100px">Zone</th>
              <th style="width: 150px">Client</th>
              <th>IP</th>
            </tr>
            </thead>

            <tbody v-if="Object.keys(filteredClientList).length > 0"
                   style="padding: 30px; overflow-y: scroll !important">

            <tr v-for="(client, index) in filteredClientList.slice().reverse().slice(0, listLimitSize)" :key="index">
              <td class="w-10" style="text-align:center">
                <div class="avatar-list avatar-list-stacked">
                  <img class="avatar-img rounded-circle" style="width:25px" :src="getClassImage(client.class)">
                  <img class="avatar-img rounded-circle" style="width:25px" :src="getRaceImage(client.race)">
                </div>
              </td>
              <td style="align-content: center">{{client.name}}</td>
              <td>{{client.level}}</td>
              <td>
            <span v-if="client.server && client.server.zone_name">
              {{client.server.zone_name}}
              <span class="badge badge-soft-primary">{{client.server.zone_id}} ({{client.server.instance_id}})</span>
            </span>
                <span v-if="!client.server && client.online == 1">
              Character Select
            </span>
                <span v-if="!client.server && client.online > 0">
              Zoning
            </span>
              </td>
              <td>
            <span v-if="client.client_version">
              {{eqClientVersionConstants[client.client_version]}}
            </span>
              </td>
              <td>
            <span v-if="client.ip">
              {{intToIP(client.ip)}}
            </span>
              </td>
            </tr>
            </tbody>
          </table>

        </div>


        <div class="card-body" v-if="!clientList && loaded">
          There are currently no players online...
        </div>

      </div>
    </div>
  </div>

</template>

<script>
  import {EqemuAdminClient} from '@/app/core/eqemu-admin-client'
  import eqClassIconConstants from '@/app/core/eq-class-icon-constants'
  import eqClassIntToStringConstants from '@/app/core/eq-class-int-to-string-constants'
  import eqRaceIconConstants from '@/app/core/eq-race-icon-constants'
  import eqClientVersionConstants from '@/app/core/eq-client-version-constants'
  import Timer from '@/app/core/timer'

  import util from 'util'

  export default {
    name: 'PlayersOnline',

    props: {
      fullList: {
        default: false,
        required: false
      }
    },

    data() {
      return {
        listLimitSize: null,
        loaded: false,
        refreshing: false,
        eqClassIconConstants: {},
        eqRaceIconConstants: {},
        eqClientVersionConstants: {},
        clientList: null,
        filteredClientList: null,
        playersOnlineChart: null,
        playerSearchFilter: null
      }
    },

    mounted() {
      this.listLimitSize = (this.fullList ? 10000 : 50)
    },

    methods: {

      filterPlayers() {
        if (!this.playerSearchFilter) {
          this.filteredClientList = this.clientList;
          return;
        }

        let clients = [];
        this.clientList.forEach(client => {
          const filter = this.playerSearchFilter.toLowerCase();

          if (
            client.name.toLowerCase().includes(filter) ||
            (client.server && client.server.zone_long_name.toLowerCase().includes(filter)) ||
            (client.server && client.server.zone_name.toLowerCase().includes(filter)) ||
            (client.ip && this.intToIP(client.ip).includes(filter))
          ) {
            clients.push(client);
          }
        })

        this.filteredClientList = clients;
      },

      /**
       * @param num
       */
      intToIP: num => {
        var d = num % 256
        for (var i = 3; i > 0; i--) {
          num = Math.floor(num / 256)
          d   = num % 256 + '.' + d
        }

        var ip = d.split('.')

        return ip[3] + '.' + ip[2] + '.' + ip[1] + '.' + ip[0]
      },

      /**
       * @param classId
       * @returns {string|any}
       */
      getClassImage: classId => {
        if (eqClassIconConstants[classId]) {
          return require('@/assets/img/icons/classes-races/item_' + eqClassIconConstants[classId] + '.png')
        }

        return 'data:image/gif;base64,R0lGODlhAQABAIAAAMLCwgAAACH5BAAAAAAALAAAAAABAAEAAAICRAEAOw=='
      },

      /**
       * @param raceId
       * @returns {string|any}
       */
      getRaceImage: raceId => {
        if (eqRaceIconConstants[raceId]) {
          return require('@/assets/img/icons/classes-races/item_' + eqRaceIconConstants[raceId] + '.png')
        }

        return 'data:image/gif;base64,R0lGODlhAQABAIAAAMLCwgAAACH5BAAAAAAALAAAAAABAAEAAAICRAEAOw=='
      },

      /**
       * Builds online player list
       *
       * @returns {boolean}
       */
      async buildPlayersOnlineList() {
        this.refreshing     = true
        const apiClientList = await EqemuAdminClient.getWorldClientList()
        this.refreshing     = false
        if (!apiClientList) {
          this.clientList = null
          return false
        }

        var clientList = []
        if (apiClientList.length > 0) {
          apiClientList.forEach(function (row) {
            if (row.character_id === 0) {
              return
            }
            clientList.push(row)
          })
        }

        if (clientList.length === 0) {
          this.clientList = null
          return false
        }

        this.clientList = clientList
        this.filterPlayers()
      }
    },

    /**
     * Destroy
     */
    beforeDestroy() {
      clearInterval(Timer.timer['players-online'])
    },

    /**
     * Create
     *
     * @returns {Promise<void>}
     */
    async created() {

      await this.buildPlayersOnlineList()

      this.loaded = true
      var self    = this

      if (Timer.timer['players-online']) {
        clearInterval(Timer.timer['players-online'])
      }

      Timer.timer['players-online'] = setInterval(function () {
        if (!document.hidden) {
          self.buildPlayersOnlineList()
        }
      }, 5000)

      /**
       * Classes Online breakdown
       */
      let classCounts = []
      if (this.clientList && this.clientList.length > 0) {
        this.clientList.forEach(function (row) {
          const classId = parseInt(row.class)
          if (typeof classCounts[classId] === 'undefined') {
            classCounts[classId] = 0
          }
          classCounts[classId]++
        })
      }

      let classCountsDataColumn  = []
      let classCountsColumnNames = {}
      for (let i = 1; i <= 16; i++) {
        const classCount = (typeof classCounts[i] !== 'undefined' ? classCounts[i] : 0)
        if (classCount > 0) {
          classCountsDataColumn.push([eqClassIntToStringConstants[i], classCount])
          classCountsColumnNames[eqClassIntToStringConstants[i]] = eqClassIntToStringConstants[i]
        }
      }

      this.eqClassIconConstants     = eqClassIconConstants
      this.eqRaceIconConstants      = eqRaceIconConstants
      this.eqClientVersionConstants = eqClientVersionConstants

    }
  }

</script>

<style scoped>
  .players-online td {
    padding:        0.4rem;
    padding-top:    0.5rem;
    padding-right:  0.5rem;
    padding-bottom: 0.5rem;
    padding-left:   0.5rem;
  }
</style>
