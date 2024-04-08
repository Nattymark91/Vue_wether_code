<template>

<div class="app">

    <weather-form
         @create="createItem"
    />

    <weather-list
     :items="items" 
     @remove="removeItem"
     />
     
</div>
    
</template>

<script>
import WeatherForm from "@/components/WeatherForm.vue";
import WeatherList from "@/components/WeatherList.vue";
import DeleteButton from './components/UI/DeleteButton.vue';
import ShowButton from './components/UI/ShowButton.vue';

export default {
    components: {
        WeatherList, 
        WeatherForm, 
        DeleteButton,
        ShowButton
    },
    data() {
        return {
            items: [
                {id: 1, city: 'Москва'}
            ],
        }
        
    },
    methods: {
        createItem(item) {
            this.items.push(item);
            this.updateStorageList();         
        },
        removeItem(item) {
            this.items = this.items.filter(i => i.id !== item.id);
            this.updateStorageList();
        },
        updateStorageList() {
            localStorage.cityArray = JSON.stringify(this.items);
        }
    },
    mounted() {
        if (localStorage.cityArray) {
            this.items = JSON.parse(localStorage.cityArray);
        }
    }
}
</script>

<style >

* {
	padding: 0px;
	margin: 0px;
	border: none;
}

.app {
    background: rgb(2,0,10);
    background: linear-gradient(0deg, rgba(2,0,10,1) 29%, rgba(18,40,110,1) 100%);
    width: 100%;
    height: 100vh;
    overflow: auto;
}

</style>
