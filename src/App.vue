<template>
  <div class="container">
    <div style="margin: 0 0 30px 0;">
      <span v-for="category in categories" :key="category.id" class="category-title" @click="selectCategory(category)">{{category}}</span>
    </div>
    <div>
    <div v-for="item in categoryData" :key="item.id" class="card-container">
      <div>
        <img :src="item.icon_url" alt="">
      </div>
      <div class="card-container__details">
        <div style="color: red" class="card-padding">{{ selectedCategory }}</div>
        <div style="color: grey" class="card-padding">{{ item.created_at }}</div>
        <div class="card-container__pill card-padding">{{ item.id }}</div>
        <div class="card-container__pill card-padding">{{ item.updated_at }}</div>
        <div class="card-container__pill card-padding">{{ item.value }}</div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import {ref, onMounted, watch} from 'vue';
export default {
  setup () {
    const categories = ref([])
    const selectedCategory = ref(null)
    const categoryData = ref([])

    watch(selectedCategory, () => {
      fetchCategoryData(selectedCategory)
    })

    onMounted(async () => {
      await fetchCategories()
    })

    function selectCategory(category){
      selectedCategory.value =category
    }

    async function fetchCategories(){
      const {data} = await axios.get('https://api.chucknorris.io/jokes/categories')
      categories.value = data
    }

    async function fetchCategoryData(selectedCategory){
      const {data} = await axios.get(`https://api.chucknorris.io/jokes/search?query=${selectedCategory.value}`)
      categoryData.value = data.result
    }

    return {
      categories,
      selectCategory,
      categoryData
    }
  }
}
</script>

<style lang="css" scoped>
.category-title{
  padding: 10px;
    margin: 10px;
    background-color: grey;
    border-radius: 15px;
    cursor: pointer;
    font-size: 10px;
}

.category-title:hover{
    background-color: yellow;
    color: black;
}

.card-container{
  display: flex;
    flex-direction: row;
    align-content: center;
    justify-content: center;
    font-size: 10px;
    align-items: center;
  background-color: white;
  margin: 10px 0;
}

.card-container__details{
  text-align: left;
    margin: 15px;
}

.card-padding{
  padding: 10px 0;
}

.card-container__pill{
  background-color: grey;
    width: fit-content;
    margin: 5px 0;
    border-radius: 5px;
    border-radius: 5px;
}

</style>