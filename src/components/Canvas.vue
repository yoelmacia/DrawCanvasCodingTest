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
    drawShapes(pathElement, element) {
      let canvas = document.getElementById("canvas");
      let context = canvas.getContext("2d");
      context.beginPath();
      context.fillStyle = element.color;
      context.stroke(pathElement);
      context.fill(pathElement);
      context.closePath();
      this.elements.push({
        id: element.id,
        name: element.label,
        path: pathElement,
        coordinates: element.coordinates
      });
      this.nameCoordinate();
    },
    drawCircle(x, y, radius) {
      let canvas = document.getElementById("canvas");
      let context = canvas.getContext("2d");
      context.fillStyle = "#FF0000";
      context.beginPath();
      context.arc(x, y, radius, 0, 2 * Math.PI);
      context.fill();
    },
    drawHandles() {
      const closeEnough = 10;
      this.dataArray.forEach(element => {
        element.coordinates.forEach((coordinate, index) => {
          if (element.id === 1) {
            if (index === 0) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 1) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 2) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            }
          }
          if (element.id === 2) {
            if (index === 0) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 1) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 2) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 3) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            }
          }
          if (element.id === 3) {
            if (index === 0) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 1) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 2) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            } else if (index === 3) {
              this.drawCircle(coordinate.x, coordinate.y, closeEnough);
            }
          }
        });
      });
    },
    createImage() {
      let canvas = document.getElementById("canvas");

      let path = new Path2D();
      let path2 = new Path2D();
      let path3 = new Path2D();

      canvas.addEventListener("click", event => {
        this.handleMouseClick(event);
      });

      canvas.addEventListener("mousedown", event => {
        this.handleMouseDown(event);
      });

      this.drawHandles();

      this.dataArray.forEach(element => {
        // for triangle
        if (element.id === 1) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              path.moveTo(coordinate.x, coordinate.y);
              path.lineTo(coordinate.x + 100, coordinate.y + 150);
            } else if (index === 1) {
              path.lineTo(coordinate.x - 50, coordinate.y + 100);
            } else if (index === 2) {
              path.lineTo(coordinate.x - 50, coordinate.y - 250);
            }
          });
          this.drawShapes(path, element);
        }
        // for retangle
        if (element.id === 2) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              path2.moveTo(coordinate.x, coordinate.y);
              path2.lineTo(coordinate.x + 100, coordinate.y);
            } else if (index === 1) {
              path2.lineTo(coordinate.x, coordinate.y + 100);
            } else if (index === 2) {
              path2.lineTo(coordinate.x - 100, coordinate.y);
            } else if (index === 3) {
              path2.lineTo(coordinate.x, coordinate.y - 100);
            }
          });
          this.drawShapes(path2, element);
        }
        // new rectangle
        if (element.id === 3) {
          element.coordinates.forEach((coordinate, index) => {
            if (index === 0) {
              path3.moveTo(coordinate.x, coordinate.y);
              path3.lineTo(coordinate.x + 50, coordinate.y);
            } else if (index === 1) {
              path3.lineTo(coordinate.x, coordinate.y + 50);
            } else if (index === 2) {
              path3.lineTo(coordinate.x - 50, coordinate.y);
            } else if (index === 3) {
              path3.lineTo(coordinate.x, coordinate.y - 50);
            }
          });
          this.drawShapes(path3, element);
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
    handleMouseDown(e) {
      let canvas = document.getElementById("canvas");
      let offsetLeft = canvas.getBoundingClientRect().left;
      let offsetTop = canvas.getBoundingClientRect().top;
      let mouseX = e.pageX - offsetLeft;
      let mouseY = e.pageY - offsetTop;
      this.elements.forEach(element => {
        element.coordinates.map(coordinate => {
          if (
            this.checkCloseEnough(coordinate.x, mouseX) &&
            this.checkCloseEnough(coordinate.y, mouseY)
          ) {
            console.log(coordinate.name);
          }
        });
      });
    },
    checkCloseEnough(p1, p2) {
      return Math.abs(p1 - p2) < 10;
    },
    nameCoordinate() {
      this.elements.forEach(element => {
        element.coordinates.map((coordinate, index) => {
          coordinate.name = `${element.name}${index}`;
          return coordinate;
        });
      });
    },
    deleteShape(selected) {
      let canvas = document.getElementById("canvas");
      let context = canvas.getContext("2d");
      this.dataArray.forEach((element, index) => {
        if (element.label === selected) {
          this.dataArray.splice(index, 1);
          context.clearRect(0, 0, canvas.width, canvas.height);
          this.createImage();
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
