<template>
  <v-app>
    <v-card>
      <v-layout>
        <v-app-bar color="info">
          <div class="d-flex">
            <v-app-bar-nav-icon @click="rail = false"></v-app-bar-nav-icon>
          </div>

          <v-app-bar-title>Logo</v-app-bar-title>
          <v-spacer></v-spacer>
          <div>
            <v-responsive class="d-none d-sm-flex mr-5" width="300">
              <v-text-field
                density="compact"
                variant="outlined"
                label="Search"
                append-inner-icon="mdi-magnify"
                single-line
                hide-details></v-text-field>
            </v-responsive>
          </div>
        </v-app-bar>
        <v-navigation-drawer
          v-model="drawer"
          :rail="rail"
          permanent
          @click="rail = false">
          <v-list-item
            prepend-avatar="https://randomuser.me/api/portraits/men/85.jpg"
            title="Muhammad Afaq"
            nav>
            <template v-slot:append>
              <v-btn
                variant="text"
                icon="mdi-chevron-left"
                @click.stop="rail = !rail"></v-btn>
            </template>
          </v-list-item>

          <v-divider></v-divider>

          <v-list density="compact" nav>
            <v-list-item
              prepend-icon="mdi-home-city"
              title="Home"
              to="/"></v-list-item>
            <v-list-item
              prepend-icon="mdi-dip-switch"
              title="Result"
              to="/result"></v-list-item>
            <v-list-item
              prepend-icon="mdi-information"
              title="About"
              to="/about"></v-list-item>
            <v-list-item class="d-flex d-sm-none">
              <v-responsive class="mx-auto" width="300">
                <v-text-field
                  density="compact"
                  variant="outlined"
                  label="Search"
                  append-inner-icon="mdi-magnify"
                  single-line
                  hide-details></v-text-field>
              </v-responsive>
            </v-list-item>
          </v-list>
        </v-navigation-drawer>
        <v-main style="min-height: 100vh">
          <router-view />
        </v-main>
      </v-layout>
    </v-card>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      drawer: true,
      rail: true,
    };
  },
};
</script>
