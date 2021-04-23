<template>
  <header>
    <img src="../assets/logo.svg" alt="logo" />
    <div class="search-container" @click="$emit('show-search')">
      <input
        type="text"
        class="location"
        :value="location"
        placeholder="Select a location"
        readonly
      />

      <input
        type="text"
        class="guests"
        :value="`${guests == 0 ? '' : `${guests} guests`}`"
        placeholder="Add guests"
        readonly
      />
      <span class="material-icons"> search </span>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    location: {
      type: String,
    },
    guests: {
      type: Number,
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/variables.scss";
@import "../styles/extraStyles.scss";

header {
  margin-bottom: 4.5rem;

  img {
    margin-bottom: 4rem;
  }

  .search-container {
    display: flex;
    align-items: center;

    margin: 0 3rem;
    border-radius: 16px;
    box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);

    * {
      padding: 2rem 1.6rem;
    }

    input {
      flex-grow: 1;
      border: none;
      color: #333333;
      font-size: 1.6rem;
      text-align: center;
    }

    .guests {
      border-left: 1px solid #f2f2f2;
      border-right: 1px solid #f2f2f2;
    }

    span {
      cursor: pointer;
      color: rgba($primary-color, 0.9);
    }
  }
}

@media screen and (min-width: $breakpoint-tablet) {
  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}

@media screen and (min-width: $breakpoint-desktop) {
  header {
    img {
      margin: 0;
    }

    .search-container {
      margin: 0;

      &focus-within {
        border: 1px solid #333333;
      }
    }
  }
}
</style>