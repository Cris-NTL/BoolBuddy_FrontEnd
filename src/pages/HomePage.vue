<script>
import axios from "axios";
import ProfileCard from "../components/ProfileCard.vue";
import { store } from "../store";
import { VueperSlides, VueperSlide } from 'vueperslides';
import 'vueperslides/dist/vueperslides.css';
import { resolveDirective } from "vue";
import { routerKey } from "vue-router";

export default {

  name: "HomePage",

  data() {
    return {
      store,
      profiles: [],
      technologies: [],
      selectedTechnology: "",
    };
  },

  props: {
    profile: Object,
  },

  components: {
    VueperSlides,
    VueperSlide,
    ProfileCard,
  },

  computed: {
    imageUrl() {
      return `${this.store.baseUrl}/storage${profile.photo}`;
    },
  },

  mounted() {
    this.getTechnologies();
    this.getCarouselProfiles();
  },

  methods: {
    getProfiles() {
      this.store.selectedTechnology = this.selectedTechnology;
      this.$router.push({ name: "search" });
    },
    getTechnologies() {
      axios.get(`${this.store.baseUrl}/api/technologies`).then((resp) => {
        this.technologies = resp.data.results;
        console.log(resp.data.results);
      })
    },
    getCarouselProfiles() {
      axios.get(`${this.store.baseUrl}/api/profiles`).then(
        (resp) => {
          this.profiles = resp.data.results.data;
        },
      );
    },
  },

};
</script>

<template>
  <!-- Jumbotron -->
  <div class="jumbotron">
    <div class="title text-center">

      <p class="fs-4 pt-5 mx-5">BoolBuddy offers professional IT services, with our experts specializing in a wide
        range of fields including web development, full-stack development, junior web development, web design and
        much more. Whether you're starting a new project or need help with an existing one, we're here to assist
        you.</p>

      <h1 class="display-5 fw-bold">
        Find Your IT Specialist
      </h1>

      <div class="col">
        <label class="form-label" for="technology">Technologies</label>
        <select v-model="selectedTechnology" id="technology" class="form-select w-50 m-auto" @change="getProfiles()">
          <option selected value=""></option>
          <option value="all">All</option>
          <option v-for="technology_item in technologies" :key="technology_item.id" :value="technology_item.id">{{
            technology_item.name }}</option>
        </select>
      </div>

    </div>
  </div>

  <!-- carousel made with vueperslides -->
  <h3 class="pt-5 text-center">Meet the newcomers!</h3>
  <vueper-slides class="no-shadow" :visibleSlides="3" slide-multiple :slidePerView="3" :gap="5" :bullets="false"
  :arrows="true" :breakpoints="{ 800: { visibleSlides: 1, slideMultiple: 1 } }" :touchable="false">
  <vueper-slide class="slide" v-for="profile in profiles" :key="user_id" :title="profile.name"
  :content="profile.surname"
  :image="profile.photo ? `${this.store.baseUrl}/storage${profile.photo}` : 'src/assets/defaultimg/profilejpg.jpg'">
</vueper-slide>

</vueper-slides>
</template>

<style lang="scss">
@use '../assets/Style/AppHomePage.scss';

.no-shadow {
  .slide {
    opacity: 0.9;
    border-radius: 1rem !important;
  }

  .vueperslide {

    &__title {
      color: white;
      font-size: 1.2rem;
      text-transform: uppercase;

    }

    &__content {
      color: white;
      font-weight: bold;
      font-size: 1.2rem;
      text-transform: uppercase;
      border-radius: 1rem !important;
    }

    &__content-wrapper {
      background-color: transparent;
      background-color: rgba(0, 0, 0, 0.452);
      border-radius: 1rem;
    }

    .vueperslide {
      border-radius: 10px !important;
      background: none;
      box-shadow: none;
      border-radius: 1rem !important;
    }

  }
}</style>