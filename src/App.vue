<template>
  <div id="app">
    <main>
      <nav>
        <span>&nbsp;</span>
        <router-link to="/petropolis/oil" v-bind:class="{active: $route.name && $route.name.match('petropolis')}" @click.native="routeClick('/petropolis/oil')">Petropolis</router-link>
        <span>&nbsp;</span>
        <div v-on:click="scrollToAside" id="aside-scroll-to" title="Skip To Content">
          <i class="material-icons">keyboard_arrow_down</i>
        </div>

      </nav>
      <router-view name="map" id="map" />
      <v-dialog name="request-location">
      </v-dialog>
      <modals-container/>
    </main>
    <AppNav></AppNav>
    <aside v-bind:class="{ 'no-flex': this.asideHidden}">
      <div id="aside-wraper">
        <div v-on:click="toggleAside" id="aside-toggle" title="Toggle Content">
          <i v-if="asideHidden" class="material-icons">menu</i>
          <i v-else class="material-icons">close</i>
        </div>
        <p id="aside-heading" v-bind:class="{hidden: this.asideHidden}">it's all fossil fuels</p>
      </div>
      <div id="content" v-bind:class="{hidden: this.asideHidden}" ref="asideContent">
        <router-view/>
      </div>
    </aside>
  </div>
</template>

<style>
#app {
  /* Steven 1/18 */
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
/*  font-family:  'Roboto', Arial, and Monospace; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex: 1 0 auto; /* 2 */
  flex-direction: column;
}

nav {
  height: 60px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #dc143c;      /*navbar background color */
}
nav a {
  font-weight: bold;
  color: #4F1D2A;              /*navbar text color */
  text-decoration: none;
  max-height: 56px;
  display: flex;
  align-items: center;
  font-size: 1em;            /*no idea what this does */
  padding: 5px 50px ;
  line-height: 38px;
}
nav a.active, nav a.router-link-exact-active {
  background-color: #FFFFFF;       /*background color when active */
  color: #333333;                    /*text color when active */
  font-size: 310%;
  margin: 10px;
}
nav a:hover {
  color: black;
}
#map {
  background-color: #f7fcfe;
  height: calc(100vh - 60px);
  /* border-top: thin solid #dc143c;         /* Its lower border of navbar*/
  box-sizing: border-box;
}

aside {
  background-color: #f7fcfe;              /*background of text panel*/
  border-left: solid #dc143c;      /* Its left-hand border of text panel*/
  border-bottom: medium solid #dc143c;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  overflow: visible;
}

#aside-wraper {
  position: sticky;
  top: 0;
  color: #f7fcfe;                               /*toggle x color*/
  background-color: #dc143c;               /*background of sidebar closer square*/
  z-index: 1;
  height: 60px;
}

#aside-toggle, #aside-scroll-to {
  cursor: pointer;
  padding-top: 20px;
  text-align: center;
  height: 0px;
  width: 100%;
  max-width: 50px;
  float: right;
  display: none;
}
#aside-toggle:hover, #aside-scroll-to:hover {
  color: #ab0107;
  background-color: white;
  cursor: hand;
}

#aside-scroll-to {
  display: block;
}

aside .fullwidth {
  width: 100%;
}

.no-flex{
  position: absolute;
  top: 0;
  right: 0;
  height: 50px;
  width: 50px;
}
.hidden {
  display: none;
}

#content {
  max-height: 100%;
  overflow-y: visible;
  text-align: justify;
  color: black;                /*sidebar text color*/
}

#aside-heading {
  margin-top: 0.40em;
  padding: 0;
  text-align: center;
  font-size: 180%;             /* sidebar header title, "Learning" */
  color: white;

}

#content {
  padding: 0 1em;
}

.aside-content h1{             /* sidebar item title settings */
  margin-top: 1em;
  margin-bottom: 1em;
  color: #800000;
}

.caption {
text-align: center; font-style: italic; font-size: 90%; margin-top:5px; margin-bottom:8px;
}

.quote {
font-weight: bold; font-style: italic; color: #333333;
}

@media (min-width: 1075px) {
  nav a {
    font-size: 1.5em;
    padding: 5px 50px;
  }
}

@media (min-width: 850px) {
  #app {
    flex-direction: row;
    height: 100vh;
  }
  main {
    flex: 1;
    margin: 0;
  }
  nav {
    justify-content: space-around;
  }
  aside {
    flex: 0 0 400px;
  }
  #aside-heading {
    padding: 0 0.6em;
    text-align: CENTER;
  }
  #aside-toggle {
    display: block;
  }
  #aside-scroll-to {
    display: none;
  }
  #content, aside {
    overflow-x: hidden;           /*these scrolls affect hamburger button at upper right! */
    overflow-y: auto;        /*they create weird micro arrows, very strange */
    max-height: 100vh;
  }
}

@media (max-width: 850px) {
  nav a {
    font-size: 1.25em;
  }
}
@media (max-width: 500px) {
  nav a {
    font-size: 1em;
    padding: 5px 5px;
  }
}

</style>

<script>
import { mapGetters } from 'vuex'
import AppNav from './components/AppNav.vue'
import {eventBus} from './main'
import {doInfoPopUp} from './components/InfoPopUp'

export default {
  name: 'App',
  components: {
    AppNav
  },
  watch: {
    '$route' (to, from) {
      // react to route changes...
      document.querySelector('aside > #content').scrollTop = 0
    }
  },
  computed: {
    // mix the getters into computed with object spread operator
    ...mapGetters([
      'asideHidden'
    ])
  },
  mounted () {
    // Check to see if the infoPopUp query string was added to the route and display popup if so.
    if (this.$route.query && this.$route.query.infoPopUp) {
      doInfoPopUp(this, this.$route.query.infoPopUp, false)
    }
  },
  methods: {
    toggleAside () {
      this.$store.dispatch('toggle')
    },
    scrollToAside () {
      this.$refs.asideContent.scrollIntoView()
    },
    routeClick: function (to) {
      if (to === this.$route.path) {
        eventBus.$emit('route-click')
      }
    }
  }
}

</script>
