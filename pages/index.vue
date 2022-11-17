<template>
  <main>
    <div class="intro">
      <h1>Nuxt Toolbox</h1>
      <p>
        Hi 👋!!!! This template gives you a
        <a href="https://nuxtjs.org/">Nuxt</a> app with the scaffolding for
        <a href="https://www.netlify.com/products/functions/"
          >Netlify Functions</a
        >, <a href="https://www.netlify.com/products/forms/">Forms</a>, and
        <a href="https://docs.netlify.com/routing/redirects/">Redirects</a>. Our
        aim was to give you the code you would need to hit the ground running
        with a few fun features.
      </p>

      <p>
        You can find the code for this project on GitHub at
        <a href="https://github.com/netlify-templates/nuxt-toolbox"
          >https://github.com/netlify-templates/nuxt-toolbox</a
        >! Happy coding!
      </p>
      <p>
        <button @click="serverlessTest()">Test</button>
        {{serverlessResponse}}
      </p>
      {{ user }}

      <p v-if="user.id">
        <button @click="logout()">Logout {{user.email}}</button>
      </p>
      <p v-else>
        <button @click="login()">Login</button>
      </p>

    </div>
    <FeedbackForm />
    <JokeBlock />
  </main>
</template>
<!-- no ts! -->
<script setup>
import netlifyIdentity from 'netlify-identity-widget'
import {ref, onMounted} from "vue";
const serverlessResponse = ref('');

const serverlessTest = async () => {
  const response = await fetch('/.netlify/functions/test').then(response => response.json());
  serverlessResponse.value = response.message;
}

const user = ref({});

const login = () => {
  console.log('login function');
  netlifyIdentity.open('login');
};

onMounted(() => {
  netlifyIdentity.init({ locale: 'de'});
  netlifyIdentity.on('login', u => {
    console.log('login event', u)
    user.value = u;
  });
  netlifyIdentity.on('logout', u => {
    console.log('logout event', u)
    user.value = {};
  });
})

const logout = () => {
  console.log('logout function');
  netlifyIdentity.logout();
};

</script>
<style>
* {
  font-family: 'Helvetica', sans-serif;
}
.intro {
  min-width: 400px;
  padding: 10px 40px;
  width: 60%;
}
</style>
