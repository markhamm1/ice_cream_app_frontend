<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      <p>Name: <input v-model="newFlavorName"></p>
      <p>Ingredients: <input v-model="newFlavorIngredients"></p>
      <p>Color: <input v-model="newFlavorColor"></p>
      <p>Image Url: <input v-model="newFlavorImageUrl"></p>
      <button v-on:click="createFlavor">Add Flavor</button>
    </div>

    <div v-for="flavor in flavors">
      <hr>
      <p>Name: {{ flavor.name }}</p>
      <!-- <p>Ingredients: {{ flavor.ingredients }}</p>
      <p>Name: {{ flavor.color }}</p> -->
      <img v-bind:src="flavor.image_url" style="width:300px;">
      <br>
      <button v-on:click="showFlavor(flavor)">More Info</button>
    </div>

    <dialog id="show-flavors">
      <form method="dialog">
        <h1>{{ currentFlavor.name }}</h1>
        <p>Name: {{ currentFlavor.name }}</p>
        <p>Ingredients: {{ currentFlavor.ingredients }}</p>
        <p>Color: {{ currentFlavor.color }}</p>
        <p>Image Url: {{ currentFlavor.image_url }}</p>
        <img v-bind:src="currentFlavor.image_url" style="width:600px;">
        <br>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Ice Cream!!!",
      flavors: [],
      currentFlavor: {},
      newFlavorName: "",
      newFlavorIngredients: "",
      newFlavorColor: "",
      newFlavorImageUrl: "",
    };
  },
  created: function () {
    this.indexFlavors();
  },
  methods: {
    indexFlavors: function () {
      console.log("flavors index...");
      axios.get("api/flavors").then((response) => {
        console.log("api/flavors", response);
        this.flavors = response.data;
      });
    },

    showFlavor: function (flavor) {
      console.log("more info...");
      console.log(flavor);
      this.currentFlavor = flavor;
      document.querySelector("#show-flavors").showModal();
    },

    createFlavor: function () {
      console.log("creating flavor...");
      var params = {
        name: this.newFlavorName,
        ingredients: this.newFlavorIngredients,
        color: this.newFlavorColor,
        image_url: this.newFlavorImageUrl,
      };
      axios
        .post("api/flavors", params)
        .then((response) => {
          console.log("api/flavors", response);
          this.flavors.push(response.data);
        })
        .catch((errors) => {
          this.errors = errors.response;
        });
    },
  },
};
</script>