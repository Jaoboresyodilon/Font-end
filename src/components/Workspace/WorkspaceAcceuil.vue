<script setup>
// Importation des plugins personnalisés
import { Cookies } from "@/plugins/cookies";
import { RestApi } from "@/plugins/restApi";
import { Scroll } from "@/plugins/scroll";

// Importation des plugins Vue.js
import { onBeforeMount } from "vue";

// Importation des plugins Pinia
import { useAlertStore } from "@/pinia/alert";
import { usePersonneStore } from "@/pinia/personne";
import { useStatusStore } from "@/pinia/status";

// Instances des plugins
const cookies = new Cookies();
const restApi = new RestApi();
const scroll = new Scroll();

// Instances des stores Pinia
const alert = useAlertStore();
const personne = usePersonneStore();
const status = useStatusStore();

// Chargement des données avant le montage
onBeforeMount(() => {
  scroll.toTop();
  restApi
    .get(`/api/personne/${cookies.get("idPersonne")}`)
    .then((response) => {
      personne.set(response.data);
    })
    .catch((error) => {
      alert.error();
      console.error(error);
    });
});
</script>

<template>
  <v-container class="acceuil-container">
    <v-row>
      <v-col cols="12" class="justify-center">
        <div class="content-flot">
        <h3 class="text-center text-h5 mt-9 ">
          <strong>Bienvenue <span class="text-weigt-bold">{{ personne.nom }} {{ personne.prenoms }} !</span></strong>
        </h3>
      </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <div class="content-box">
          <ul>
            <li class="my-4">
              Bienvenue sur la plateforme de gestion des données de l'ESP
              Antsiranana.
            </li>
            <li class="my-4" >
              Cette plateforme a été conçue afin de faciliter vos activités
              pédagogiques.
            </li>
            <li class="my-4">
              Vous faites partie des :
              <ol class="ml-10">
                <li
                  v-for="(item, index) in status.list"
                  :key="index"
                  class="my-2"
                >
                  <strong>{{ item }}</strong>
                </li>
              </ol>
            </li>
            <li class="my-4">
              Les menus sur la barre de navigation correspondent à vos statuts.
            </li>
          </ul>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped>
.acceuil-container {
  background-image: url('img/fond4.png'); /* Image de fond */
  background-size: cover;
  background-position: center;
  min-height: 90vh;
  font-size: 1.2rem;
  color: #0C0A0AFF; /* Texte en blanc pour contraste */
  padding: 2rem;
}

.content-box {
  background-color: rgba(241, 233, 233, 0.6); /* Superposition sombre semi-transparente */
  padding: 1.5rem;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.3);
}
.content-flot {
  background-color: rgba(241, 233, 233, 0.6); /* Superposition sombre semi-transparente */
  padding: 0.1rem;
  border-radius: 5px;
  box-shadow: 0px 1px 10px rgba(0, 0, 0, 0.3);
  max-width: 400px ;
  margin: 50px ;
  text-align: center;
  margin: 0 auto ;
}

.text-shadow {
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7); /* Ajout d'ombre au texte */
}

ul, li, ol {
  line-height: 1.8; /* Meilleure lisibilité du texte */
  font-weight: 560; /* Pour mettre le texte en gras */
}

@media screen and (max-width: 768px) {
  .acceuil-container {
    padding: 1rem;
  }

  .content-box {
    padding: 1rem;
  }
}
</style>
