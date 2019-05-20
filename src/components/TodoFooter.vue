<template>
    <div class="clearAllContainer">
        <!--        <span class="clearAllBtn" v-on:click="clearTodo">clear all</span>-->
        <mdb-btn class="clearAllBtn" color="danger" v-on:click="clearTodo">clear all</mdb-btn>
    </div>
</template>

<script>
    import {mdbBtn} from 'mdbvue';

    export default {
        methods: {
            clearTodo() {
                this.$emit('removeAll');
            },
        },
        components: {
            mdbBtn
        }
    }
</script>

<style scoped>
    .clearAllContainer {
        width: fit-content;
        height: fit-content;
        line-height: 50px;
        background-color: white;
        border-radius: 5px;
        margin: 0 auto;
        cursor: pointer;
    }

    .clearAllBtn {
        color: #ffffff;
    }
</style>
