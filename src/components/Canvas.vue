<template>
  <div>
    <canvas id="canvas" width="700" height="500"></canvas>
    <button @click="createImage()">Create</button>
    <button @click="createRandomRectangle()">Create Random Rectangle</button>
    <div>
      Selected: {{selected}}
      <button @click="deleteShape(selected)">Delete</button>
    </div>
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
      url: "https://about-you-pangea.s3.eu-central-1.amazonaws.com/shapes.json",
      elements: [],
      selected: ""
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

      let path = new Path2D();
      let path2 = new Path2D();
      let path3 = new Path2D();

      canvas.addEventListener("click", event => {
        this.handleMouseClick(event);
      });

      this.dataArray.forEach((element, index) => {
        // for triangle
        if (index === 0) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              path.moveTo(coordinate.x, coordinate.y);
              // path.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path.lineTo(coordinate.x + 100, coordinate.y + 150);
            } else if (index === 1) {
              //path.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path.lineTo(coordinate.x - 50, coordinate.y + 100);
            } else if (index === 2) {
              //path.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path.lineTo(coordinate.x - 50, coordinate.y - 250);
            }
          });
          context.beginPath();
          context.fillStyle = element.color;
          context.stroke(path);
          context.fill(path);
          context.closePath();
          this.elements.push({
            id: element.id,
            name: element.label,
            path: path
          });
        }
        // for retangle
        if (index === 1) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              path2.moveTo(coordinate.x, coordinate.y);
              //path2.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path2.lineTo(coordinate.x + 100, coordinate.y);
            } else if (index === 1) {
              //path2.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path2.lineTo(coordinate.x, coordinate.y + 100);
            } else if (index === 2) {
              //path2.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path2.lineTo(coordinate.x - 100, coordinate.y);
            } else if (index === 3) {
              //path2.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path2.lineTo(coordinate.x, coordinate.y - 100);
            }
          });
          context.beginPath();
          context.fillStyle = element.color;
          context.stroke(path2);
          context.fill(path2);
          context.closePath();
          this.elements.push({
            id: element.id,
            name: element.label,
            path: path2
          });
        }
        // new rectangle
        if (index === 2) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              path3.moveTo(coordinate.x, coordinate.y);
              //path3.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path3.lineTo(coordinate.x + 50, coordinate.y);
            } else if (index === 1) {
              //path3.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path3.lineTo(coordinate.x, coordinate.y + 50);
            } else if (index === 2) {
              //path3.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path3.lineTo(coordinate.x - 50, coordinate.y);
            } else if (index === 3) {
              //path3.rect(coordinate.x - 5, coordinate.y - 5, 10, 10);
              path3.lineTo(coordinate.x, coordinate.y - 50);
            }
          });
          context.beginPath();
          context.fillStyle = element.color;
          context.stroke(path3);
          context.fill(path3);
          context.closePath();
          this.elements.push({
            id: element.id,
            name: element.label,
            path: path3
          });
        }
      });
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

      this.elements.forEach(element => {
        if (context.isPointInPath(element.path, mouseX, mouseY)) {
          this.selected = element.name;
        }
      });
    },
    deleteShape(selected) {
      console.log(this.dataArray.length);
      this.dataArray.forEach((element, index) => {
        if (element.label === selected) {
          this.dataArray.splice(index, 1);
          // re-render canvas
        }
      });
      console.log(this.dataArray.length);
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
