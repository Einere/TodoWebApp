<template>
    <div class="hello">

        <mdb-input label="Material input" v-model="value" placehoder="title"/>
    </div>
</template>

<script>
    import {mdbInput} from 'mdbvue';

    export default {
        name: 'HelloWorld',
        data() {
            return {
                msg: 'Welcome to Your MDB Vue.js App',
                value: "",
            };
        },
        components: {
            mdbInput: mdbInput
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        font-weight: normal;
        padding-top: 20px;
        padding-bottom: 30px;
    }

    p {
        color: #969696;
        margin-bottom: 0;
        font-size: 14px;
    }
</style>
