<template>
  <b-container fluid>
    <b-row>
      <b-col class="outerWrapper mt-md-5" cols="12" md="10" offset-md="1">
        <b-row class="title-row bg-info">
          <b-col cols="7" class="title">
            <p>TWITCH STREAMERS</p>
          </b-col>
          <b-col cols="5">
            <div id="filters">
              <input type="radio" id="all" value="all" v-model="filter" />
              <label for="all" class="btn btn-primary">ALL</label>
              <input type="radio" id="online" value="online" v-model="filter" />
              <label for="online" class="btn btn-primary">ONLINE</label>
              <input type="radio" id="offline" value="offline" v-model="filter" />
              <label for="offline" class="btn btn-primary">OFFLINE</label>
            </div>
          </b-col>
        </b-row>
        <div class="results">
          <div
            class="row result-row mt-2"
            v-for="result in filteredResults(results)"
            :key="result.id"
          >
            <div class="col-6 col-md-3">
              <img src :alt="result.name" :srcset="result.logo" class="logo" />
            </div>
            <div class="col-6 col-md-3">
              <a :href="result.link" target="_blank" rel="noopener noreferrer"></a>
              {{result.name}}
            </div>
            <div class="col-md-6 d-none d-md-block text-truncate">
              <span>{{result.status}}</span>
            </div>
          </div>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import axios from "axios";
export default {
  name: "TwitchBoard",
  data() {
    return {
      filter: "all",
      results: []
    };
  },
  mounted() {
    let vm = this;
    let initTarget = [
      "ESL_SC2",
      "OgamingSC2",
      "cretetion",
      "freecodecamp",
      "storbeck",
      "habathcx",
      "RobotCaleb",
      "noobs2ninjas"
    ];
    initTarget.forEach(item => {
      let url = `https://wind-bow.glitch.me/twitch-api/channels/${item}`;
      axios
        .get(url)
        .then(response => {
          let data = response.data;
          let result = {
            status: data.status ? data.status : "offline",
            logo: data.logo,
            name: data.display_name,
            id: data._id,
            link: data.url
          };
          vm.results.push(result);
        })
        .catch(i => console.log(i));
    });
  },
  methods: {
    filteredResults(list) {
      switch (this.filter) {
        case "all":
          return list;
        case "online":
          return list.filter(item => item.status !== "offline");
        case "offline":
          return list.filter(item => item.status === "offline");
      }
      return;
    }
  }
};
</script>

<style lang="scss">
.outerWrapper {
  background: lightgray;

  .title-row {
    .title {
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 3rem;

      @media (max-width: 768px) {
        font-size: 1.5rem;
      }

      p {
        margin-bottom: 0;
      }
    }

    #filters {
      position: relative;
      display: flex;
      flex-direction: column;
      justify-content: space-evenly;
      align-items: center;

      input[type="radio"] {
        position: absolute;
        top: -9999px;
        left: -9999px;

        &:checked + label {
          background: #27ae60;
        }
      }
      label {
        display: block;
        width: 100px;
        margin: 0.5rem 0;
      }
    }
  }

  .results {
    .result-row {
      background: #2c3e50;
      color: #bbbbbb;
      text-align: left;
      line-height: 50px;
      height: 50px;

      .logo {
        max-width: 50px;
        max-height: 50px;
        border-radius: 50%;
        border: 3px solid #e1e1e6;
      }
    }
  }
}
</style>