<script setup>
// my plugins
import { URL } from '@/plugins/url';
import { Service } from '@/plugins/service';

// vuejs's plugins
import { ref } from 'vue';

// pinia's plugins
import { useAlertStore } from '@/pinia/alert';
import { useAuthentificationStore } from '@/pinia/authentification';

// instance my plugins
const url = new URL();
const service = new Service();

// instance pinia's plugins
const alert = useAlertStore();
const authentification = useAuthentificationStore();

// create some variable as ref
const pseudo = ref('');
const motDePasse = ref('');
const showPassword = ref(false);
const images = ref([
  'img/fond1.jpg',
  'img/fond2.jpg',
  'img/fond3.jpg',
]);

// verifying the form and try to login
function verify() {
  // make form's data on an object
  let requestData = {
    pseudo: pseudo.value,
    motDePasse: motDePasse.value
  }
  
  // check if form is OK
  if (service.verifyFormIfOK(requestData)) {
    authentification.check(service.stringTrim(pseudo.value), service.stringTrim(motDePasse.value));
  } 
  else alert.warningForm();
}

const logoUrl = 'img/logo-esp-antsiranana.png';
const logoUnaUrl = 'img/logo-una.png';

</script>

<template>
  <div id="authentification">
    <header>
      <div class="header-top">
        <img class="logo-una" :src="logoUnaUrl" alt="Logo UNA" />
        <div class="header-title">ECOLE SUPÉRIEURE POLYTECHNIQUE D'ANTSIRANANA</div>
        <img class="logo" :src="logoUrl" alt="Logo ESP" />
      </div>
    </header>
    <main>
      <div class="breadcrumb"></div>
      <div class="main-content">
        <v-carousel class="main-image" hide-delimiters :cycle="true" :interval="3000">
          <v-carousel-item v-for="(image, i) in images" :key="i" :src="image"></v-carousel-item>
        </v-carousel>
        <div class="login-form">
          <h2>Bienvenue sur GNS-ESP</h2>
          <form autocomplete="off" @submit.prevent>
            <v-text-field
              type="text"
              label="Votre Identifiant ou e-mail"
              hide-details="auto"
              class="mb-6"
              clearable
              v-model="pseudo"
              prepend-inner-icon="mdi-account"
            ></v-text-field>
            <v-text-field
              :append-inner-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
              :type="showPassword ? 'text' : 'password'"
              label="Votre mot de passe"
              hide-details="auto"
              class="mb-6"
              clearable
              v-model="motDePasse"
              autocomplete="off"
              prepend-inner-icon="mdi-lock"
              @click:append-inner="showPassword = !showPassword"
              @keypress.enter="verify"
            ></v-text-field>
            <v-btn
              append-icon="mdi-login"
              block
              rounded="lg"
              size="x-large"
              color="primary"
              class="text-transform-class text-none"
              @click="verify"
            >Se connecter</v-btn>
          </form>
        </div>
      </div>
    </main>
  </div>
</template>
<style scoped>
#authentification {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #F4F6F9;
  color: #333;
  min-height: 100vh;
}

.header-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #07335F;
  color: #FFF;
  padding: 10px 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.logo, .logo-una {
  height: 80px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.header-title {
  flex: 1;
  text-align: center;
  font-size: 24px;
  font-weight: bold;
  text-transform: uppercase;
}

.breadcrumb {
  padding: 15px 20px;
  background-color: #E9ECEF;
  margin-top: 10px;
  border-radius: 5px;
  font-size: 14px;
}

.main-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30px;
}

.main-image {
  width: 700px;
  height: auto;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  margin-left: 40px;
}

.login-form {
  background-color: #FFF;
  padding: 40px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  width: 400px;
  border-radius: 10px;
  position: relative;
  margin-right: 30px;
}

.login-form h2 {
  margin-bottom: 30px;
  font-size: 22px;
  background-color: #07335F;
  color: white;
  padding: 15px;
  border-radius: 5px;
  text-align: center;
  text-transform: uppercase;
}

.login-footer {
  margin-top: 20px;
  text-align: center;
}

/* Media Queries */
@media screen and (max-width: 1024px) {
  .main-content {
    flex-direction: column;
    align-items: center;
  }

  .login-form {
    margin-left: 0;
    margin-top: 60px;
  }

  .main-image {
    width: 100%;
  }
}

@media screen and (max-width: 768px) {
  .header-title {
    font-size: 20px;
  }

  .login-form {
    width: 90%;
    padding: 20px;
  }
}
</style>
