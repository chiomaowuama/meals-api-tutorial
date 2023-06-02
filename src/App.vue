<script setup>
import { onMounted, ref } from 'vue';

const catMeals = ref(null);
const meals = ref(null);



async function getCategories() {
  let categories = await fetch('https://www.themealdb.com/api/json/v1/1/categories.php');
  let meals = await categories.json();

  catMeals.value = meals.categories.map((catMeal) => {
    return{
      name: catMeal.strCategory,
      picture: catMeal.strCategoryThumb,
      description: catMeal.strCategoryDescription,
    };
  });

  // console.log(catMeals.value);
}

async function viewMeals(name) {
  let apiMeals = await fetch('https://www.themealdb.com/api/json/v1/1/filter.php?c=' + name);
  apiMeals = await apiMeals.json();
  
  meals.value = apiMeals.meals.map(meal => {
    return {
      id: meal.idMeal,
      name: meal.strMeal,
      image: meal.strMealThumb
    }
  });
}
function goBack(){
  meals.value = null;
}
// onMounted(() => {
//   getCategories();
// })
</script>

<template>
  <header class="the-background">
    <div class="the-cover">
      <div v-if="!meals">
        <div v-if="!catMeals">
          <button @click="getCategories()" class="big-btn">View categories</button>
        </div>
        <div v-else>
        <table border="1px" class="first-table">
          <thead>
            <tr>
              <td>id</td>
              <td>categories</td>
              <td>categoriesThumb</td>
              <td>description</td>
              <td></td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(catMeal,index) in catMeals" :key="catMeal.name">
              <td>{{ index + 1 }}</td>
              <td>{{ catMeal.name }}</td>
              <td><img :src="catMeal.picture" alt="catImage" class="imagespic"></td>
              <td>{{ catMeal.description }}</td>
              <td><button @click="viewMeals(catMeal.name)" class="tinybtn">View Meals</button></td>
            </tr>
          </tbody>
        </table>
        </div>
      
      </div>
      <div v-else>
        <button @click="goBack()" class="tinybtn">Back</button>
        <table border="1"  class="first-table">
          <thead>
            <tr>
              <td>S/N</td>
              <td>Image</td>
              <td>ID</td>
              <td>Name</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(meal, index) in meals" :key="meal.id">
              <td>{{ index + 1 }}</td>
              <td><img :src="meal.image" alt="catImage" class="imagespic1"></td>
              <td>{{ meal.id }}</td>
              <td>{{ meal.name }}</td>

              
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </header>

  <RouterView />
</template>

<style >
*{
  margin:0px;
}
.the-background{
  background-image: url("assets/bg.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  Width:100%;
  min-height:100vh;
  margin:0%;
  position:relative;
}
.big-btn{
  border:2px solid rgba(255, 0, 0, 0.16);
  background-color:rgba(255, 0, 0, 0.397);
  box-shadow: 2px;
  color:rgb(196, 190, 190);
  font-weight: 500;
  border-radius: 10px;
  font-size:25px;
  text-transform: capitalize;
  padding:10px 25px;
  position:absolute;
  top:45%;
  left:20%;
}
.big-btn:hover{
  color:white;
  background-color:rgba(255, 0, 0, 0.605);
}
.the-cover{
  background-color:rgba(0, 0, 0, 0.516);
  Width:100%;
  min-height:100vh;
}
.first-table{
  border:2px solid black;
  width:100%;
}
.first-table thead tr td, .first-table tbody tr td{
  padding:10px;
  color:rgb(229, 223, 223);
  font-size:15px;
  font-family:Georgia, 'Times New Roman', Times, serif;
  line-height: 1.5rem;
  font-weight: 400;
  text-align: center;
 
}
.first-table thead tr td{
  text-align: center;
  font-size:17px;
  text-transform: uppercase;
}
.imagespic{
  width:80%;
  margin-left:10%;
}
.imagespic1{
  width:180px;
  margin-left:2.5%;
  border-radius:15px;
}
.tinybtn{
  border:2px solid rgba(255, 0, 0, 0.16);
  background-color:rgba(255, 0, 0, 0.397);
  box-shadow: 2px;
  color:rgb(196, 190, 190);
  font-weight: 500;
  border-radius: 10px;
  font-size:13px;
  text-transform: capitalize;
  padding:5px 20px;
}
.tinybtn:hover{
  color:white;
  background-color:rgba(255, 0, 0, 0.605);
}
@media (min-width: 1024px) {
  
}
</style>
