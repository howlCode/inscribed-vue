<template>
  <section>
    <nav class="navbar is-dark" aria-label="main navigation">
      <div class="navbar-brand">
        <i class="far fa-star"></i><router-link class="logo-link" to="/"><h1 id="logo">Inscribed</h1></router-link>
        <a role="button" class="navbar-burger" @click="showNav = !showNav" :class="{ 'is-active': showNav }" aria-label="menu" aria-expanded="false">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
      <div class="navbar-menu" :class="{ 'is-active': showNav }">
        <div class="navbar-start">
          <router-link class="navbar-item" to="/new-arcs"><i class="far fa-thumbs-up nav-icon has-text-primary"></i>Vote on New Story-Arcs!</router-link>
          <router-link class="navbar-item" to="/stories"><i class="far fa-eye nav-icon has-text-info"></i>See All Stories</router-link>
          <router-link class="navbar-item" to="/new-story"><i class="fas fa-edit nav-icon has-text-danger"></i>Create a New Story</router-link>
        </div>
        <div class="navbar-end">
          <div class="navbar-item">
            <button v-if="$isSignedIn()" class="button is-light" @click="signOut()">Sign out</button>
            <router-link v-if="!$isSignedIn()" class="button is-primary" to="/signin">Sign in</router-link>
          </div>
        </div>
      </div>
      <h2>{{ error }}</h2>
    </nav>
  </section>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      error: "",
      showNav: false
    };
  },
  methods: {
    signOut() {
      this.$http.secured
        .delete("/signin")
        .then(response => {
          this.$store.commit("unsetCurrentUser");
          this.$router.replace("/");
        })
        .catch(error => this.setError(error, "Cannot sign out"));
    },
    setError(error, text) {
      this.error =
        (error.response && error.response.data && error.response.data.error) ||
        text;
    }
  }
};
</script>

<style lang="css">
.navbar {
  margin-bottom: -10px;
}
.navbar-brand h1 {
  margin-right: 50px;
}
.spacer {
  margin-top: 20px;
}
.nav-pages {
  margin-bottom: -30px;
}
.fa-star {
  margin-top: 5px;
  margin-left: 10px;
  margin-right: 10px;
}
.nav-icon {
  margin-right: 10px;
}
@media (max-width: 1087px) {
  .nav-icon {
    display: none;
  }
}
</style>