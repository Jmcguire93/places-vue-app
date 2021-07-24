<template>
  <div class="home">
    <h2>Add a place</h2>
    <div>
      Name:
      <input type="text" v-model="newPlaceParams.name" />
      Address:
      <input type="text" v-model="newPlaceParams.address" />
    </div>
    <button v-on:click="createPlace()">Create Place</button>
    <h1>{{ message }}</h1>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>Name: {{ place.name }}</h2>
      <p>Address: {{ place.address }}</p>
      <button v-on:click="showPlace(place)">More info!</button>
    </div>
    <dialog id="place-details">
      <form method="dialog">
        <h1>Place Info!</h1>
        <p>Name: {{ currentPlace.name }}</p>
        <p>Address: {{ currentPlace.address }}</p>

        <button v-on:click="updatePlace(currentPlace)">Update!</button>
        <button v-on:click="destroyPlace(currentPlace)">Delete!</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Places",
      places: [],
      newPlaceParams: {},
      currentPlace: {},
      errors: [],
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("http://localhost:3000/places/").then((response) => {
        console.log("All Places:", this.places);
        this.places = response.data;
      });
    },
    createPlace: function () {
      console.log("Adding place..");

      axios
        .post("http://localhost:3000/places", this.newPlaceParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.places.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      var editPlaceParams = place;
      axios.patch("http://localhost:3000/places/" + place.id, editPlaceParams).then((response) => {
        console.log("Update successful!", response.data);
      });
    },
    destroyPlace: function (place) {
      axios.delete("http://localhost:3000/places/" + place.id).then((response) => {
        console.log("Deleted!", response.data);
        var index = this.place.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>
