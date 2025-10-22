<script>
    export default { 
        // Add code in appropriate places
        data() {
            return {
                items: ['keyboard', 'mouse', 'iPhone', 'macbook', 'adapter'],
                newItem: ''   
            }
            
        },
    methods: {
        addItem() {
            if (this.newItem.trim() !== '') {
                this.items.push(this.newItem.trim());
                this.newItem = ''; 
            }
        },
        deleteItem(index) {
            this.items.splice(index, 1); 
    }
}
}
    
</script>

<template>

    <h2>Shopping Cart</h2>
    <!-- TODO: Add Code Here -->
    <ul>
        <li v-for="(item, index) in items" :key="index">
            {{ item }}
            <button @click="deleteItem(index)">Delete</button>
        </li>
    </ul>
    <input v-model="newItem" placeholder="Enter item name">
    <button @click="addItem">Add!</button>
   
</template>


<style scoped>
h2 {
    font-family: Arial, sans-serif;
}
ul {
    list-style: none;
    padding: 0;
}
li {
    margin: 5px 0;
}
input {
    margin-right: 8px;
}
button {
    margin-left: 8px;
}
</style>
