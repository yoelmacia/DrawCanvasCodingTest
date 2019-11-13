<template>
  <div>
    <canvas id="canvas" width="700" height="500"></canvas>
    <button @click="createImage()">Create</button>
    <button @click="createRandomRectangle()">Create Random Rectangle</button>
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
  created() {
    this.fetchData();
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
      context.fillStyle = "red";
      this.dataArray.forEach((element, index) => {
        // for triangle
        if (index === 0) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x + 100, coordinate.y + 150);
            }
            if (index === 1) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y + 100);
            }
            if (index === 2) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y - 250);
            }
            context.stroke();
          });
        }

        // for retangle
        if (index === 1) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x + 100, coordinate.y);
            }
            if (index === 1) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y + 100);
            }
            if (index === 2) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 100, coordinate.y);
            }
            if (index === 3) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y - 100);
            }
            context.stroke();
          });
        }
        // new rectangle

        if (index === 2) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x + 50, coordinate.y);
            }
            if (index === 1) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y + 50);
            }
            if (index === 2) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y);
            }
            if (index === 3) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y - 50);
            }
            context.stroke();
          });
        }
      });
    },
    createRandomRectangle() {
      const newRectangle = {
        id: 3,
        coordinates: [
          { x: 50, y: 50 },
          { x: 100, y: 50 },
          { x: 100, y: 100 },
          { x: 50, y: 100 }
        ],
        color: "#B39DFC",
        label: "A new rectangle"
      };
      if (this.dataArray.length === 2) {
        this.dataArray.push(newRectangle);
        this.createImage();
      }
    }
  }
};
</script>

<style></style>
