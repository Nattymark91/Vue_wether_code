<template>

<div class="list" v-if="items.length > 0">
    <weather-item 
    v-for="item in items" 
    :key="item.id"
    :item="item"
    @remove="$emit('remove', item)"
    />
</div>

<h2 v-else>Список городов пуст</h2>


    
</template>

<script>
import WeatherItem from '@/components/WeatherItem.vue'


export default {
    components: {WeatherItem},
    props: {
        items: {
            type: Array, 
            required: true
        }
    }
}

</script>

<style scoped>
.list {
    
    margin: 0 auto;
    margin-top: 15px;
    width: 70%;
}

h2 {
    text-align: center;
    color: grey;
}


</style>