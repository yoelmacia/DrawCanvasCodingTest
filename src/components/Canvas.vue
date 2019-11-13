<template>
  <div>
    <canvas id="canvas" width="700" height="500"></canvas>
    <button @click="fetchData()">Fetch</button>
    <button @click="createImage()">Create</button>
    {{ dataArray }}
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Canvas",
  data() {
    return {
      dataArray: [],
      url: "https://about-you-pangea.s3.eu-central-1.amazonaws.com/shapes.json"
    };
  },
  methods: {
    fetchData() {
      axios(this.url)
        .then(response => {
          this.dataArray = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    createImage() {
      const canvas = document.getElementById("canvas");
      const context = canvas.getContext("2d");
      this.dataArray.forEach((element, index) => {
        // for triangle
        if (index === 0) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.beginPath();
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x, coordinate.y, 10, 10);
              context.lineTo(coordinate.x + 100, coordinate.y + 150);
              context.stroke();
            }
            if (index === 1) {
              context.beginPath();
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x, coordinate.y, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y + 100);
              context.stroke();
            }
            if (index === 2) {
              context.beginPath();
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x, coordinate.y, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y - 250);
              context.stroke();
            }
          });
        }

        // for retangle
        if (index === 1) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.beginPath();
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x, coordinate.y, 10, 10);
              context.lineTo(coordinate.x + 100, coordinate.y);
              context.stroke();
            }
            if (index === 1) {
              context.beginPath();
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x, coordinate.y, 10, 10);
              context.lineTo(coordinate.x, coordinate.y + 100);
              context.stroke();
            }
            if (index === 2) {
              context.beginPath();
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x, coordinate.y, 10, 10);
              context.lineTo(coordinate.x - 100, coordinate.y);
              context.stroke();
            }
            if (index === 3) {
              context.beginPath();
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x, coordinate.y, 10, 10);
              context.lineTo(coordinate.x, coordinate.y - 100);
              context.stroke();
            }
          });
        }
      });
      //context.strokeRect(50, 50, 50, 50);
    }
  }
};
</script>

<style></style>
