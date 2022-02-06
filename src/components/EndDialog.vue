<template>
  <v-dialog
		v-model="dialog"
		max-width="400px"
		@click:outside="onClickClose"
		@keydown="onClickClose"
    >
      <!-- <v-card>
        <v-card-title class="text-h5">
          Use Google's location service?
        </v-card-title>

        <v-card-text>
          Let Google help apps determine location. This means sending anonymous location data to Google, even when no apps are running.
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            color="green darken-1"
            text
            @click="dialog = false"
          >
            Disagree
          </v-btn>

          <v-btn
            color="green darken-1"
            text
            @click="dialog = false"
          >
            Agree
          </v-btn>
        </v-card-actions>
      </v-card> -->
      <v-card class ="pb-10">
        <v-toolbar>
          <v-toolbar-title>Word of the Day</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn
            icon
            @click="onClickClose"
          >
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card
          class="mx-auto"
          max-width="400"
          tile
          elevation="0"
          v-if="def != null"
        >
          <v-subheader>{{def.word.toUpperCase()}}</v-subheader>
          <v-list-item >
            <v-list-item-content>
              <v-list-item-subtitle>
                {{def.phonetics[0].text}}
              </v-list-item-subtitle>
              <v-list-item-title>
                <audio controls ref="mediaElement">
                  <source :src="'https:' + def.phonetics[0].audio" />
                   <!-- :src= def.phonetics[0].audio.slice(2) > -->
                </audio>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item v-for="(meaning, i) in def.meanings" :key="'def'+i">
            <v-list-item-content two-line>
              <v-list-item-subtitle> {{meaning.partOfSpeech}} </v-list-item-subtitle>
              <v-list-item-title class="ml-4 white-space">{{"" + meaning.definitions[0].definition}}</v-list-item-title>
              <v-list-item-subtitle class="ml-4 white-space">{{"Example: " + meaning.definitions[0].example}}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
          
        </v-card>

      </v-card>
    </v-dialog>
</template>

<script>
import definitionJson from "../data/definition.json";
// const app_id = "56ebff64";
// const app_key = "754468355f12a31a834dd3f839393559";
// const fields = "pronunciations,definitions,domains,examples,regions";
// const strictMatch = "false";
export default {
  name: "EndDialog",
  props: {
    dialog: {
      type: Boolean,
      required: true
    },
    word: {
      type: String
    }
  },
  async mounted() {
		console.log("mounted")
    console.log(definitionJson)
		// const baseUrl = 'https://api.dictionaryapi.dev/api/v2/entries/en/' + this.word.toLowerCase();
	// const options = {
	// 	url: '/api/v2/entries/en-gb/' + this.word + '?fields=' + fields + '&strictMatch=' + strictMatch,
	// 	headers: {
	// 		'app_id': app_id,
	// 		'app_key': app_key
	// 	}
	// };
    // let headers= {
    //   'Access-Control-Allow-Origin' : '*',
    //   'Access-Control-Allow-Methods':'GET,PUT,POST,DELETE,PATCH,OPTIONS',
    //   'Access-Control-Allow-Credentials':true
    // };
		// await this.$http
    //   .get(baseUrl, {headers})
    //   .then(result => {
    //     console.log(result);
    //   })
    //   .catch(e => console.error("error found", e));
  },
  data: () => ({
    def: definitionJson[0]
  }),

  methods: {
    onClickClose() {
		this.$emit("dialogClose", false);
    }
  }

}
</script>

<style>
.white-space {
  white-space: inherit !important;
}

</style>