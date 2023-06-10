<template>
  <div>
    <form class="row" @submit.prevent="updateColor">
      <input
        id="color-input"
        v-model.number="colorCount"
        placeholder="Количество цветов"
      /><br>
      <button name="create-button" type="submit">Generate</button>
      <button name="save-button" @click="saveAsImage">Save as IMG</button>
    </form>
    <div>
      <div id="mesh" :style="meshStyle"></div>
    </div>
  </div>
</template>

<script>
import domtoimage from 'dom-to-image';


export default {
  data() {
    return {
      meshStyle: {
        width: '1920px',
        height: '1080px',
        backgroundColor: '',
        backgroundImage: '',
      },
      colorCount: 7,
    };
  },
  methods: {
    getRandomColor() {
      const hue = Math.floor(Math.random() * 360);
      const saturation = 100;
      const lightness = 75;
      return `hsla(${hue}, ${saturation}%, ${lightness}%, 1)`;
    },
    updateColor() {
      const backgroundImages = [];

      for (let i = 0; i < this.colorCount; i++) {
        const color = this.getRandomColor();
        const gradient = `radial-gradient(at ${Math.random() * 100}% ${Math.random() * 100}%, ${color} 0px, transparent 50%)`;
        backgroundImages.push(gradient);
      }

      this.meshStyle.backgroundImage = backgroundImages.join(', ');
      this.meshStyle.backgroundColor = this.getRandomColor();
    },
    saveAsImage() {
  const meshElement = document.getElementById('mesh');
  meshElement.style.width = `1920px`;
  meshElement.style.height = `1080px`;

  const options = {
    width: outputWidth,
    height: outputHeight,
  };

  domtoimage.toPng(meshElement, options)
    .then(dataUrl => {
      const link = document.createElement('a');
      link.download = 'gradient.png';
      link.href = dataUrl;
      link.click();
    })
    .catch(error => {
      console.error('Error saving image:', error);
    });

  meshElement.style.width = '1920px';
  meshElement.style.height = '720px';
},



  },
  mounted() {
    this.updateColor();
  },
};
</script>
