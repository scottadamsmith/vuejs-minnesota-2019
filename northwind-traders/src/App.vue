<template>
  <div id="app">
    <header>
      <nav-bar :user="auth.currentUser"></nav-bar>
    </header>

    <b-container>
      <b-row>
        <b-col>
          <main role="main" class="flex-shrink-0">
            <div class="container">
              <router-view />
            </div>
          </main>
        </b-col>
        <b-collapse id="collapseNotifications" class="border-left pl-2">
          <b-col>
            <notification-panel></notification-panel>
          </b-col>
        </b-collapse>
      </b-row>
    </b-container>

    <footer class="footer mt-auto py-3">
      <div class="container">
        <span class="text-muted">
          Northwind Traders &copy; {{ new Date() | date('YYYY') }} - Build:
          {{ release.build }} - Environment: {{ release.environment }} - Failed
          Health Checks:
          {{ failedHealthCheckCount }}
        </span>
      </div>
    </footer>
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import NotificationPanel from '@/components/NotificationPanel.vue'
import { AuthService } from '@/services/NorthwindService.js'
import { mapState, mapGetters } from 'vuex'

export default {
  name: 'app',
  components: {
    NavBar,
    NotificationPanel
  },
  data() {
    return {
      auth: Object
    }
  },
  mounted() {
    this.auth = AuthService
    AuthService.token()
  },
  computed: {
    ...mapState(['release', 'healthChecks']),
    ...mapGetters(['failedHealthCheckCount'])
  }
}
</script>

<style lang="scss">
@import './assets/custom.scss';
@import '~bootswatch/dist/Cosmo/variables';
@import '~bootstrap/scss/bootstrap';
@import '~bootswatch/dist/Cosmo/bootswatch';
@import '~bootstrap-vue/dist/bootstrap-vue';
</style>

<style>
html,
body {
  height: 100%;
}

#app {
  display: flex;
  flex-direction: column;
  height: 100%;
}

main > .container {
  padding: 8px 15px 8px 15px;
}

.footer {
  background-color: #f5f5f5;
}

.footer > .container {
  padding-right: 15px;
  padding-left: 15px;
}
.feather {
  width: 16px;
  height: 16px;
}

#collapseNotifications {
  width: 30%;
}
</style>
