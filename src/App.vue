<template>
  <Header
    :location="searchQuery.currentLocation"
    :guests="guests"
    @show-search="isSearching = true"
  />

  <transition name="slide-down">
    <Search
      :adults="searchQuery.adults"
      :children="searchQuery.children"
      :currentLocation="searchQuery.currentLocation"
      :allLocations="getConcatedLocations"
      @close="isSearching = false"
      @search="search($event)"
      v-show="isSearching"
    />
  </transition>

  <main>
    <section class="stays">
      <div class="stays__header">
        <h2>Stays in Finland</h2>
        <span
          >{{ staysCount > 12 ? "12+" : staysCount }}
          {{ staysCount == 1 ? "stay" : "stays" }}</span
        >
      </div>

      <div class="stays__content">
        <Stay v-for="stay in filteredStays" :key="stay.id" :stay="stay" />
      </div>
    </section>
  </main>

  <Footer />
</template>

<script>
import Stay from "./components/Stay";
import Search from "./components/Search";
import Header from "./components/Header";
import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Stay,
    Search,
    Header,
    Footer
  },
  data() {
    return {
      searchQuery: {
        adults: 0,
        children: 0,
        superHost: false,
        currentLocation: ""
      },
      allStays: [],
      allLocations: [],
      isSearching: false
    };
  },
  methods: {
    search(event) {
      this.searchQuery.currentLocation = event[0];
      this.searchQuery.children = event[1];
      this.searchQuery.adults = event[2];
      this.searchQuery.superHost = event[3];

      this.isSearching = false;
    }
  },
  computed: {
    staysCount() {
      return this.filteredStays.length;
    },
    guests() {
      return this.searchQuery.children + this.searchQuery.adults;
    },
    getConcatedLocations() {
      return this.allLocations.map(x => {
        return `${x.city}, ${x.country}`;
      });
    },
    filteredStays() {
      let filteredStays = this.allStays;

      if (this.searchQuery.currentLocation)
        filteredStays = filteredStays.filter(
          x => `${x.city}, ${x.country}` == this.searchQuery.currentLocation
        );

      if (this.guests > 0)
        filteredStays = filteredStays.filter(x => x.beds >= this.guests);

      if (this.searchQuery.superHost)
        filteredStays = filteredStays.filter(x => x.superHost);

      return filteredStays;
    }
  },
  beforeMount() {
    this.allStays = require("./stays.json");

    this.allStays.forEach(stay => {
      this.allLocations.push(stay);
    });
  }
};
</script>

<style lang="scss">
@import "./styles/variables.scss";

#app {
  min-height: 100vh;

  display: flex;
  flex-direction: column;

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  padding: $container-padding;
}

.stays {
  &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 2.9rem;

    h2 {
      color: #333333;
      font-size: 1.8rem;
    }

    span {
      font-weight: 500;
      color: #4f4f4f;
      font-size: 1.5rem;
    }
  }

  &__content {
    display: flex;
    gap: 5rem 3rem;
    flex-wrap: wrap;
    margin-bottom: 1rem;
  }
}

.slide-down-enter-active {
  animation: slide-down 0.5s;
}

.slide-down-leave-active {
  animation: slide-down 0.5s reverse;
}

@keyframes slide-down {
  0% {
    opacity: 0;
    transform: translateY(-50px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@media screen and (min-width: $breakpoint-desktop) {
  .stays {   
    &__content {
      justify-content: space-between;
    }
  }
}
</style>
