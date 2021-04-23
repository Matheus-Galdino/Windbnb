<template>
  <div id="mask">
    <section class="search">
      <header>
        <h2>Edit your search</h2>
        <span class="material-icons" @click="$emit('close')"> clear </span>
      </header>

      <div class="search__info">
        <div class="search__info__header">
          <div @click="changeContainer('location')">
            <label>Location</label>
            <input
              type="text"
              :value="selectedLocation"
              placeholder="Select a location"
              readonly
            />
          </div>

          <div class="stroke"></div>

          <div @click="changeContainer('guests')">
            <label>Guests</label>

            <input
              type="text"
              :value="`${guests == 0 ? '' : `${guests} guests`}`"
              placeholder="Add guests"
              readonly
            />
          </div>

          <button
            class="desktop-button"
            @click="
              $emit('search', [
                selectedLocation,
                childrenCount,
                adultsCount,
                superHost,
              ])
            "
          >
            <span class="material-icons">search</span>
            Search
          </button>
        </div>

        <section class="filters-container">
          <section
            :class="['locations-container', { active: isSelectingLocation }]"
          >
            <p
              v-for="location in uniqueLocations"
              :key="location"
              @click="selectedLocation = location"
            >
              <span class="material-icons"> location_on </span>
              {{ location }}
            </p>

            <p
              class="clear"
              @click="selectedLocation = ''"
              v-show="selectedLocation != ''"
            >
              <span class="material-icons">clear</span>
              Clear location
            </p>
          </section>

          <section :class="['guests-container', { active: isEditingGuests }]">
            <div>
              <h3>Adults</h3>
              <small>Ages 13 or above</small>
              <div class="count">
                <span
                  class="material-icons"
                  @click="adultsCount = adultsCount == 0 ? 0 : adultsCount - 1"
                >
                  remove
                </span>
                <p>{{ adultsCount }}</p>
                <span class="material-icons" @click="adultsCount++">add</span>
              </div>
            </div>

            <div>
              <h3>Children</h3>
              <small>Ages 2-12</small>
              <div class="count">
                <span
                  class="material-icons"
                  @click="
                    childrenCount = childrenCount == 0 ? 0 : childrenCount - 1
                  "
                  >remove
                </span>
                <p>{{ childrenCount }}</p>
                <span class="material-icons" @click="childrenCount++">add</span>
              </div>
            </div>
          </section>

          <section class="checkbox-container">
            <Checkbox
              text="Superhost"
              backgroundColor="#eb5757"
              color="#fff"
              @change="superHost = $event"
            />
          </section>
        </section>
      </div>

      <button
        class="mobile-button"
        @click="
          $emit('search', [
            selectedLocation,
            childrenCount,
            adultsCount,
            superHost,
          ])
        "
      >
        <span class="material-icons">search</span>
        Search
      </button>
    </section>
  </div>
</template>

<script>
import Checkbox from "./Checkbox";

export default {
  name: "Search",
  components: {
    Checkbox,
  },
  props: {
    allLocations: {
      type: Array,
    },
    currentLocation: {
      type: String,
    },
    children: {
      type: Number,
      default: 0,
    },
    adults: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      superHost: false,
      isEditingGuests: false,
      isSelectingLocation: true,
      adultsCount: this.adults,
      childrenCount: this.children,
      selectedLocation: this.currentLocation,
    };
  },
  methods: {
    distinct(value, index, self) {
      return self.indexOf(value) === index;
    },
    changeContainer(container) {
      if (container == "location") {
        this.isEditingGuests = false;
        this.isSelectingLocation = true;
      } else {
        this.isEditingGuests = true;
        this.isSelectingLocation = false;
      }
    },
  },
  computed: {
    uniqueLocations() {
      return this.allLocations.filter(this.distinct);
    },
    guests() {
      return this.childrenCount + this.adultsCount;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/variables.scss";
@import "../styles/extraStyles.scss";

#mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(79, 79, 79, 0.4);
}

.search {
  display: flex;
  align-items: center;
  flex-direction: column;

  height: 70vh;

  background: #fff;
  padding: $container-padding;

  //   * {
  //     font-family: "Mulish";
  //   }

  header,
  div {
    width: 100%;
  }

  header {
    display: flex;
    align-items: center;
    justify-content: space-between;

    color: #333333;
    margin-bottom: 3rem;

    h1 {
      font-size: 1.4rem;
    }

    span {
      cursor: pointer;
    }
  }

  &__info {
    &__header {
      border-radius: 16px;
      margin-bottom: 3.5rem;
      box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);

      div {
        color: #333333;
        padding: 1rem 3rem;
        cursor: pointer;

        * {
          cursor: pointer;
        }

        label {
          font-weight: 800;
          font-size: 1rem;
          display: block;
          text-transform: uppercase;
          margin-bottom: 0.5rem;
        }

        input {
          font-size: 1.4rem;
          border: none;
        }
      }

      .stroke {
        padding: 0;
        height: 1px;
        background: #f2f2f2;
      }

      .desktop-button {
        display: none;
      }
    }

    .filters-container {
      > section {
        display: flex;
        flex-direction: column;
        row-gap: 2rem;

        display: none;
        padding: 0 3rem;

        p {
          display: flex;
          column-gap: 1rem;
          align-items: center;

          color: #4f4f4f;
          font-size: 1.7rem;
          cursor: pointer;
        }

        span {
          font-size: 1.7rem;
          color: #828282;
        }

        .clear {
          color: rgba($primary-color, 1);

          span {
            color: rgba($primary-color, 1);
          }
        }
      }

      > section.active {
        display: flex;
      }

      .guests-container {
        > div {
          font-size: 1.2rem;

          h3 {
            color: #333333;
          }

          small {
            color: #bdbdbd;
          }

          .count {
            display: flex;
            column-gap: 1rem;
            align-items: center;

            margin-top: 1.5rem;

            p {
              font-weight: bold;
              font-size: 1.4rem;

              color: #333333;
            }

            span {
              border-radius: 4px;
              border: 1px solid #828282;

              padding: 0.1rem;
              cursor: pointer;
            }
          }
        }
      }

      .checkbox-container {
        margin-top: 1rem;
        cursor: initial;
        display: flex;
      }
    }
  }

  button {
    border: none;
    margin-top: auto;
    padding: 1.2rem 3rem;

    display: flex;
    color: #fff;
    column-gap: 1rem;
    align-items: center;

    border-radius: 16px;
    transition: all 0.3s ease-out;
    background: rgba($primary-color, 0.9);

    &:hover {
      background: $primary-hover-color;
    }
  }
}

@media screen and (min-width: $breakpoint-tablet) {
  .search {
    height: 40vh;

    &__info {
      &__header {
        display: flex;

        > div {
          flex: 1;

          &focus-within {
            border: 1px solid #333333;
          }
        }

        .stroke {
          max-width: 1px;
          min-height: 100%;
          height: auto;
        }

        .desktop-button {
          display: flex;
          flex-grow: 0;
          margin: 0;
        }
      }
    }

    .filters-container {
      display: flex;

      > section {
        display: flex;
        opacity: 0;
        flex: 1;
      }

      > section.active {
        opacity: 1;
      }

      .checkbox-container {
        opacity: 1;
        flex-grow: 0;
      }
    }

    button {
      display: none;
    }
  }
}
</style>