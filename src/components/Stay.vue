<template>
  <div class="stay">
    <img :src="stay.photo" alt="" />
    <div class="stay__details">
      <span class="superhost" v-if="stay.superHost">Super Host</span>
      <p>
        {{ stay.type }} . {{ stay.beds }} {{ stay.beds > 1 ? "beds" : "bed" }}
      </p>
      <p class="rating">
        <span class="material-icons">star</span>
        {{ stay.rating }}
      </p>
    </div>

    <p>
      {{ stay.title }}
    </p>
  </div>
</template>

<script>
export default {
  name: "Stay",
  props: {
    stay: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/variables.scss";

.stay {
  img {
    width: 100%;

    height: 300px;
    max-height: 300px;
    object-fit: cover;
    border-radius: 24px;
    margin-bottom: 1rem;
  }

  &__details {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
    padding: 0 0.5rem;

    .superhost {
      font-weight: bold;
      font-size: 1.3rem;
      border-radius: 9999px;
      text-transform: uppercase;

      padding: 0.7rem;
      color: #4f4f4f;
      margin-right: 1rem;
      border: 1px solid #4f4f4f;
    }

    p {
      font-weight: 500;
      font-size: 1.5rem;
      color: #828282;
    }

    .rating {
      display: flex;
      align-items: center;
      column-gap: 0.4rem;
      margin-left: auto;
      color: #4f4f4f;

      span {
        color: rgba($primary-color, 0.7);
      }
    }
  }

  > p {
    font-weight: 600;
    color: #333333;
    font-size: 1.8rem;
    padding: 0 0.5rem;
  }
}

@media screen and (min-width: $breakpoint-tablet){
  .stay{
    flex-basis: 48%;    
  }  
}

@media screen and (min-width: $breakpoint-desktop) {
  .stay {
    flex-basis: initial;
   
   img {
      max-width: 540px;
      height: 360px;
    }
  }
}
</style>