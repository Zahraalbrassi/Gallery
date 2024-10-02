<script setup>
import { ref, onMounted } from 'vue';

import PhotosData from './data.json'; // Adjust the path if necessary

const galler = ref([]);
const newUser = ref({
  img: '',
  name: '',
  description: ''
});

// Instead of using require(), create a function that resolves the path dynamically
function resolveImagePath(fileName) {
  return new URL(`./assets/images/${fileName}`, import.meta.url).href; // Properly resolve image paths in Vite/Webpack
}

onMounted(() => {
  // Loop through PhotosData and dynamically resolve the image paths
  galler.value = PhotosData.map(gal => ({
    ...gal,
    img: resolveImagePath(gal.img), // Correct the path for each image
  }));
});
function addNewUser() {
  if (newUser.value.name && newUser.value.description && newUser.value.img) {
    galler.value.push({
      id: galler.value.length + 1, // Assign a new ID based on the length of the array
      img: newUser.value.img, // Use the uploaded image URL
      name: newUser.value.name,
      description: newUser.value.description,
    });

    // Reset form after submission
    newUser.value = {
      img: '',
      name: '',
      description: ''
    };
  } else {
    alert('Please fill out all fields.');
  }
}

// Function to handle image selection from user's device
function handleImageUpload(event) {
  const file = event.target.files[0];
  if (file) {
    newUser.value.img = URL.createObjectURL(file); // Create a URL for the uploaded image
  }
}
</script>

<template>
  <h2 class="text-4xl font-serif flex items-center justify-center mb-11">PHOTO GALLERY</h2>


  <!-- Displaying the Photos data from data.json -->
 

  <div class="grid grid-cols-3 gap-4 ml-20">
    <div v-for="gal in galler" :key="gal.id" class="mb-4">
      <img :src="gal.img" alt="User Image" class="w-60 h-80 m-6 p-1" />
      <p><strong>Name:</strong> {{ gal.name }}</p>
      <p><strong>Description:</strong> {{ gal.description }}</p>
    </div>
  </div>
  <h2 class="text-4xl font-serif flex items-center justify-center mt-11">Add New Photo</h2>

  <div class="ml-20 mt-6">
    <div>
      <label for="name">Name:</label>
      <input v-model="newUser.name" type="text" placeholder="Enter name" class="border" />

      <label for="date">Description:</label>
      <input v-model="newUser.description" type="text" placeholder="Enter description" class="border" />

      <label for="img">Upload Image:</label>
      <input type="file" @change="handleImageUpload" class="border" accept="image/*" />

      <button @click="addNewUser" class="bg-blue-500 text-white px-4 py-2 mt-4">Add Photo</button>
    </div>
  </div>
</template>


