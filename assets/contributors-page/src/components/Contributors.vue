<template>
  <div>
    <v-layout row>
      <v-flex xs12 sm10 offset-sm1>
        <v-card>
          <v-subheader>Contributors: {{ totalContributors }}</v-subheader>
          <v-container fluid grid-list-sm>
            <v-layout row wrap>
              <v-flex v-for="contrib in contribs" :key="contrib.key" xs4 sm3 md2>
                <!-- <img :src="'https://github.com/' + contrib.key + '.png'" class="image" alt="lorem" width="100%" height="100%"> -->
                <ReactiveImg :src="'https://github.com/' + contrib.key + '.png'" :title="contrib.key"></ReactiveImg>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import axios from 'axios';
import ReactiveImg from './ReactiveImg';

export default {
  name: 'HelloWorld',
  components: {
    ReactiveImg
  },
  data() {
    return {
      totalContributors: ''
    };
  },
  created() {
    this.loadStats();
  },
  methods: {
    loadStats() {
      return axios
        .post('/github-stats', {
          org: 'openfaas'
        })
        .then(res => {
          const json = res.data;

          this.totalContributors = json.uniqueAuthors;
          this.contribs = Object.keys(json.byLogin)
            .map(k => {
              return { key: k, value: json.byLogin[k] };
            })
            .sort((a, b) => {
              return b.value - a.value;
            })
            .map((v, index) => {
              return {
                key: v.key,
                value: v.value,
                index
              };
            });
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
