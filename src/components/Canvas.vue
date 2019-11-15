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
      let canvas = document.getElementById("canvas");
      let context = canvas.getContext("2d");
      context.fillStyle = "red";

      canvas.addEventListener("click", event => {
        this.handleMouseClick(event);
      });

      context.beginPath();
      this.dataArray.forEach((element, index) => {
        // for triangle
        if (index === 0) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x + 100, coordinate.y + 150);
            } else if (index === 1) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y + 100);
            } else if (index === 2) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y - 250);
            }
          });
        }
        // for retangle
        if (index === 1) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x + 100, coordinate.y);
            } else if (index === 1) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y + 100);
            } else if (index === 2) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 100, coordinate.y);
            } else if (index === 3) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y - 100);
            }
          });
        }
        // // new rectangle
        if (index === 2) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              context.moveTo(coordinate.x, coordinate.y);
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x + 50, coordinate.y);
            } else if (index === 1) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y + 50);
            } else if (index === 2) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x - 50, coordinate.y);
            } else if (index === 3) {
              context.fillRect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              context.lineTo(coordinate.x, coordinate.y - 50);
            }
          });
        }
      });
      context.stroke();
    },
    // add event listener
    handleMouseClick(e) {
      let canvas = document.getElementById("canvas");
      let context = canvas.getContext("2d");
      let offsetX = canvas.getBoundingClientRect().left;
      let offsetY = canvas.getBoundingClientRect().top;
      e.preventDefault();
      let mouseX = parseInt(e.clientX - offsetX);
      let mouseY = parseInt(e.clientY - offsetY);

      let inside = context.isPointInPath(mouseX, mouseY);
      let text = inside ? "Inside" : "Outside";
      console.log(text);
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
        color: "#808080",
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
