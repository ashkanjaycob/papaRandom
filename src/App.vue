<template>
  <div>
    <!-- As a link -->
    <nav class="navbar bg-body-tertiary">
      <a class="navbar-brand p-2" href="#">PaPa4 Daste - by Ashkan</a>
    </nav>

    <div id="app">
      <div class="row">
        <div class="card bg-transparent">
          <div class="names-input card-header w-full bg-transparent">
            <div class="row d-flex justify-content-between align-items-center g-2 text-center">
              <form class="row g-3 " @submit.prevent="addNames(namesInsert)">
                <input type="text"  placeholder="Enter Player Names" class="col-12 p-3 rounded border-0" name="nameInput"
                  v-on:keyup.enter="addNames(namesInsert)" id="nameInput" v-model="namesInsert"
                  value="Mark" required />
                <div class="valid-feedback">
                  Looks good!
                </div>
                <div class="invalid-feedback">
                  Please choose a username.
                </div>
                <button type="submit" class="rounded btn btn-success col-8  col-lg-2">
                  Add
                </button>
              </form>
            </div>
          </div>

          <div class="row card-body">
            <div class="d-flex justify-content-between">
              <div class="col-7">
                <p class=""> <small> Number of teams : </small></p>
                <select class="form-select" id="selectNumberTeam" v-model="numberTeamInsert"
                  v-on:keyup.enter="randomInitialisation()"></select>
              </div>
              <button class="col-4 btn btn-success h-50 align-self-end" @click="randomInitialisation()" type="button"
                data-bs-toggle="modal" data-bs-target="#exampleModal">
                Generate
              </button>
            </div>
          </div>

          <div class="card-footer row d-flex items-center justify-between " v-for="(name, index) in namesInput"
            :key="name.content">
            <div class="col-8 name-out">
              {{ name }}
            </div>
            <button class="col-3 h-75 align-self-center btn btn-danger" @click="removeNames(index)">
              <span class="material-icons fill-current text-red-500">Delete</span>
            </button>
          </div>


          <!-- Modal -->
          <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h1 class="modal-title fs-5" id="exampleModalLabel">Teams Generated Information</h1>
                  <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body border-danger-subtle">
                  <div clas=" w-auto">
                    <div class="row ">
                      <div class="text-center shadow rounded list-none outputcss" v-for="(team, index) in teams"
                        :key="team.content">
                        <h2 v-bind:class="classObject[index]"
                          class=" rounded-t fw-bold text-center w-100 alert alert-success p-2">
                          {{ team.name }}

                        </h2>
                        <div v-for="(name) in team.members" :key="name" class="my-3 name-font">
                          {{ name }}
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="modal-footer">
                  <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                </div>
              </div>
            </div>
          </div>



        </div>
      </div>
    </div>
  </div>
</template>

<script>
var namesInput = [];
var names = [];
var teams = [];
export default {
  name: "App",
  components: {},
  data() {

    return {
      namesInput, classObject:
        ['from-blue-400 to-blue-500', 'from-red-400 to-red-500', 'from-yellow-400 to-yellow-500', 'from-green-400 to-green-500', 'from-pink-400 to-pink-500', 'from-indigo-400 to-indigo-500', 'from-purple-400 to-purple-500', 'from-green-200 to-blue-300', 'from-pink-400 to-red-500', 'from-indigo-400 to-blue-500']
      ,
      names,
      teams
    };
  },
  methods: {
    addNames: function (name) {
      //add the names in the list, clearing the input text and duplicate the name array for manipulation
      if (name != "") {
        namesInput.push(name);
        this.namesInsert = "";
        this.duplicateName();
      }
      else {
        alert("Haj Aqa khali Add Nakon!");
      }
    },
    duplicateName() {
      //duplicate the array for manipulation
      this.names = JSON.parse(JSON.stringify(namesInput));
    },
    removeNames(i) {
      //remove name by index in namesInput and update the array names for random manipulation
      namesInput.splice(i, 1);
      this.duplicateName();
    },
    randomInitialisation() {
      //vide les teams deja existante
      teams.splice(0, teams.length);
      //crée les equipes et les push dans l'array teams
      //this.numberTeamInsert = parseInt(this.numberTeamInsert)
      for (var j = 0; j < this.numberTeamInsert; j++) {
        var obj = new Object();
        obj.name = "Team" + (j + 1);
        obj.members = [];
        teams.push(obj);
      }
      //random the array names for distribution
      this.shuffle(this.names);
      //for Each neame, make a repartition in a team
      this.names.forEach((e, i) => {
        this.randomTeamRepartition(e, i);
      });
    },
    randomTeamRepartition(e, i) {
      //push the name passed in parameters in a team, /!\ all the names has been shuffled before
      teams[i % this.numberTeamInsert].members.push(e);
    },
    randomInt(max) {
      return Math.floor(Math.random() * (max - 0) + 0);
    },
    shuffle(array) {
      array.sort(() => Math.random() - 0.5);
    }
  },
  async mounted() {
    var elm = document.getElementById("selectNumberTeam"), // get the select
      df = document.createDocumentFragment(); // create a document fragment to hold the options while we create them
    for (var i = 2; i <= 6; i++) {
      // loop, i like 42.
      var option = document.createElement("option"); // create the option element
      option.value = i; // set the value property
      option.appendChild(document.createTextNode(i)); // set the textContent in a safe way.
      df.appendChild(option); // append the option to the document fragment
    }
    elm.appendChild(df); // append the document fragment to the DOM. this is the better way
  }
};
</script>
<style>
* {
  box-sizing: border-box;
}

body {
  background: url(assets/Mesii.png) !important;
  background-repeat: no-repeat;
  overflow-x: hidden;
  background-size: cover !important;
  padding-bottom: 220px;
}

.test {
  text-align: center;
}

.name-out {
  font-size: 2rem;
  font-family: 'Times New Roman', Times, serif;
  color: white;
  text-transform: capitalize;
}

.name-font {
  font-family: Arial, Helvetica, sans-serif !important;
  font-weight: bold !important;
  font-size: 1.4rem;
  color: black !important;
}

.outputcss {
  border: 2px dotted black !important;
  padding: 10px;
  margin: 10px 0;
  border-radius: 18px;
}


/*v-bind:class="{ 'bg-yellow-500':true }"  
  v-bind:style="{backgroundColor :colorBanner[index] }"*/
</style>
