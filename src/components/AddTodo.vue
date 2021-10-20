<template>
    <div>
        <form @submit="addTodo" style="margin: 8px">
            <input type="text" v-model="title" name="title" placeholder="Add Todo..">
            <input type="submit" value="Submit" class="btn">
        </form>
        
        <!-- title that inputted in above -->
        <p>{{ title }}</p>
    </div>
</template>

<script>
// import {v4} from "uuid";
export default {
    name: "AddTodo",
    data() {
        return {
            title : ''
        }
    },
    methods: {
        addTodo(e) {
            e.preventDefault();
            const newTodo = {
                title : this.title,
                completed : false
            }

            // sent up to parent 
            this.$emit("add-todo", newTodo);
            this.title = ''
        }
    }
}
</script>

<style scoped>
    form {
        display: flex;
    }
    input[type="text"] {
        flex: 10;
        padding: 5px;
    }
    input[type="submit"] {
        flex: 2;
    }
    .btn {
        background: rgb(46, 46, 70);
        color: #fff;
        border: none;
        padding: 5px 9px;
        border-radius: 2px;
        cursor: pointer;
        float: right;
    }
</style>