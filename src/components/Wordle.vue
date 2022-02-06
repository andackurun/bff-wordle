<template>
  <v-container fill-height class="d-flex flex-column align-center" @keyup.enter="keyHandler">
    <v-container>
      <div v-for="(m, index) in wordMatrix.length" :key="m">
        <v-card
        class="d-flex justify-center mb-1"
        flat
        tile
      >
        <input hidden />
        <v-card
          v-for="(char, i) in wordMatrix[index]"
          :key="index + '-' + i"
          class="pa-2 mr-2 mb-2 d-flex justify-center align-center text-h4 eachChar"
          v-bind:class="[{'filledChar': char != ''}, evaulationMatrix[index][i]]"        
          outlined
          tile
        >
          {{char != "" ? char.toUpperCase() : char}}
        </v-card>
      </v-card>
      </div>
    </v-container>
    <v-spacer />
    <v-container style="height: 200px;">
      <div style="max-width: 500px; margin: 0 auto;">
        <v-card v-for="(m, index) in keyboard.length" :key="m+'$'"
        class="d-flex justify-center mb-1"
        flat
        tile
      >
        <v-btn
          v-for="char in keyboard[index]"
          :key="m + '$' + char"
          class="pa-2 mr-2 d-flex justify-center align-center text-body-1 button normalButton"
          v-bind:class="[{'bigButton': char == 'ENTER' || char == 'delete'}, keyboardClass[char]]"
          @click="clicked(char)"
          elevation="0"
        >
        <v-icon v-if="char == 'delete'">mdi-backspace-outline</v-icon>
          <span v-if="char != 'delete'">{{char}} </span>
        </v-btn>
      </v-card>
      </div>
    </v-container>
    <EndDialog :dialog="dialog" :word="correctWord" @dialogClose="onClickClose"/>
  </v-container>
</template>

<script>
  import EndDialog from './EndDialog'
  import definitionJson from "../data/definition.json";
  export default {
    name: 'HelloWorld',
    components: {
      EndDialog
    },
    mounted() {

      let self = this; 

      window.addEventListener('keyup', function(ev) {
          self.keyHandler(ev); // declared in your component methods
      });
    },
    data: () => ({
      wordMatrix: [["","","","",""], ["","","","",""], ["","","","",""], ["","","","",""], ["","","","",""], ["","","","",""]],
      evaulationMatrix: [["","","","",""], ["","","","",""], ["","","","",""], ["","","","",""], ["","","","",""], ["","","","",""]],
      keyboard: [["Q","W","E","R","T","Y","U","I","O","P"],["A","S","D","F","H","J","K","L"],["ENTER","Z","X","C","V","B","N","M","delete"]],
      keyboardClass: {},
      wordRowNum : 0,
      wordColNum : 0,
      correctWord: definitionJson[0].word.toUpperCase(),
      dialog: false,
      status: "inprogress"
    }),
    methods: {
      keyHandler (event) {
           console.log("ev",event.key);
           if(event.keyCode >=65 && event.keyCode <= 90) {
             let pressedChar = event.key.toUpperCase();
             this.clicked(pressedChar);
           } else if(event.code == "Enter") {
             this.clicked("ENTER");
           } else if(event.code == "Backspace") {
             this.clicked("delete");
           }
      },
      clicked(char) {
        if(this.status == "finished") return;
        if(char == "ENTER") {
          this.clickedEnter();          
        } else if (char == "delete") {
          if(this.wordColNum == 0) return;
          if(this.wordColNum > 0) {
            this.wordColNum--;
          }
          let deletedRow = this.wordMatrix[this.wordRowNum];
          deletedRow[this.wordColNum] = "";
          this.$set(this.wordMatrix, this.wordRowNum, deletedRow);
          
        } else {
          if(this.wordColNum == 5) return;
          let addedRow = this.wordMatrix[this.wordRowNum];
          addedRow[this.wordColNum] = char;
          this.$set(this.wordMatrix, this.wordRowNum, addedRow);
          if(this.wordColNum < 5) {
            this.wordColNum++;
          }
        }
      },
      clickedEnter() {
        console.log("filter", this.wordMatrix[this.wordRowNum].filter(x => x == "").length)
        if(this.wordMatrix[this.wordRowNum].filter(x => x == "").length > 0) return;
        if(this.wordRowNum == 6) return;
        let guessedWord = this.wordMatrix[this.wordRowNum];
        for (let index = 0; index < guessedWord.length; index++) {
          const guessedChar = guessedWord[index];
          const correctChar = this.correctWord[index];
          console.log("char", guessedChar, correctChar);
          let className = guessedChar == correctChar ? "correct" : this.correctWord.includes(guessedChar) ? "warning" : "wrong";
          this.evaulationMatrix[this.wordRowNum][index] = className;
          this.keyboardClass[guessedChar] = className;
        }
        console.log(this.evaulationMatrix);
        this.$set(this.evaulationMatrix, this.wordRowNum, this.evaulationMatrix[this.wordRowNum]);
        this.checkFinish();
        this.wordRowNum++;
        this.wordColNum = 0;
      },
      onClickClose(value) {
        this.dialog = value;
      },
      checkFinish() {
        if(this.evaulationMatrix[this.wordRowNum].filter(x=>x == "correct").length == 5) {
          this.status = "finished";
          this.dialog = true;
        } else if (this.wordRowNum == 5) {
          this.status = "finished";
          this.dialog = true;
        }
      }
    },
  }
</script>
<style>
.eachChar {
  width: 60px;
  height: 60px;
}
.filledChar {
  border-color: gray !important;
}
.button {
  background-color: #d3d6da !important;
}
.normalButton {
  height: 58px !important; 
  width: 43px !important;
  min-width: 43px !important;
}
.bigButton {
  height: 58px !important; 
  width: 60px !important;
  min-width: 60px !important;
}
.correct {
  background-color: #6aaa64 !important;
  color: white !important;
}
.warning {
  background-color: #c9b458 !important;
  color: white !important;
}
.wrong {
  background-color: #787c7e !important;
  color: white !important;
}
.neutral {
  background-color: white !important;
}

</style>
