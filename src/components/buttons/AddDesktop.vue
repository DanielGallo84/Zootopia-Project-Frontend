<script setup>

import { ref } from 'vue';
import AnimalRepository from "../../repositories/AnimalRepository";
import AnimalService from "../../services/AnimalService";


let repository = new AnimalRepository()
let service = new AnimalService(repository)
let animalsList = []
let isLoaded = ref(false)

async function setAnimals() {
    animalsList = await service.index()
    isLoaded.value = true
}

setAnimals();

const isFormOpen = ref(false);
const newAnimalName = ref('');
const newAnimalFamily = ref('');
const newAnimalType = ref('');
const newAnimalGender = ref('');
const newAnimalDate = ref('');
const animals = ref({name: newAnimalName.value,
    photo: null,
    familyName: newAnimalFamily.value,
    typeName: newAnimalType.value,
    genderName: newAnimalGender.value,
  });

const openForm = () => {
  isFormOpen.value = true;
};

const closeForm = () => {
  isFormOpen.value = false;
};

const submitForm = () => {
  const newAnimal = {
    name: newAnimalName.value,
    photo: null,
    familyName: newAnimalFamily.value,
    typeName: newAnimalType.value,
    genderName: newAnimalGender.value,
  };

  service.create(newAnimal);
  animalsList.push(newAnimal);
  closeForm();
};

</script>

<template>
  <div id="app">
    <div id="addAnimal">
      <button @click="openForm">+ NEW ANIMAL</button>
    </div>

    <div v-if="isFormOpen" class="popup">
      <div class="popup-content">
        <form @submit.prevent="submitForm">
          <button @click="closeForm">X</button>
          <h2>NEW ANIMAL</h2>

          <div class="formGroup">
            <label for="animalName">NAME:</label>
            <input type="text" id="animalName" v-model="newAnimalName" />
          </div>

          <div class="formGroup">
            <label for="animalFamily">FAMILY:</label>
            <select id="animalFamily" v-model="newAnimalFamily">
              <option value="felids">Felids</option>
              <option value="canids">Canids</option>
              <option value="reptiles">Reptiles</option>
              <option value="mustelids">Mustelids</option>
              <option value="leporidae">Leporidae</option>
              <option value="birds">Birds</option>
            </select>
          </div>

          <div class="formGroup">
            <label for="animalType">TYPE:</label>
            <input type="text" id="animalType" v-model="newAnimalType" />
          </div>

          <div class="formGroup">
            <label for="animalGender">GENDER:</label>
            <select id="animalGender" v-model="newAnimalGender">
              <option value="female">Female</option>
              <option value="male">Male</option>
            </select>
          </div>

          <div class="formGroup">
            <label for="animalDate">DATE OF ADMISSION:</label>
            <input type="date" id="animalDate" v-model="newAnimalDate" />
          </div>

          <div class="formGroup">
            <button type="submit">SAVE</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
  
  <style lang="scss" scoped>

  #addAnimal {
    background-color: $salad;
    font-family: 'Shojumaru', system-ui;
    border-radius: 15px;
    text-align: center;
    padding: 10px;
    font-size: 15px;
  
    height: 40px;
    margin-top: 9px;
  
    button {
      color: white;
    }
  }
  
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;

    z-index: 2;

    h2 {
        font-size: 40px;
        padding: 25px;
    }
  }
  
  .popup-content {
    background-color: $light-green;
    color: white;
    font-family: 'Inter', sans-serif;
    padding: 20px;
    border-radius: 5px;
  }
  
  .formGroup {
    margin-bottom: 15px;
    display: flex;
    flex-direction: column;

  
    input {
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
  
    button {
      margin-top: 10px;
      background-color: $orange;
      border-radius: 10px;
      padding: 5px;
      color: white;
    }
  }
  </style>