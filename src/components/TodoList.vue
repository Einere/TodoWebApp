<template>
    <section>
        <transition-group name="list" tag="ul">
            <li v-for="(todoItem, index) in todoList" :key="todoItem.title" class="shadow">
                <span v-on:click="toggleSuccess(todoItem, index)">
                    <i v-if="todoItem.success" class="checkBtn far fa-check-square" aria-hidden="true"></i>
                    <i v-else class="checkBtn far fa-square" aria-hidden="true"></i>
                </span>
                <span>
                    {{ todoItem.priority }} / {{ todoItem.title }} /
                    {{ todoItem.content }}
                </span>
                <span class="expired">
                    <i v-show="todoItem.expired" class="far fa-bell" id="tooltip">
                        <span class="tooltip-text">expired</span>
                    </i>
                </span>
                <span class="editBtn" v-on:click="editTodo(todoItem, index)">
                    <i class="far fa-edit" aria-hidden="true"></i>
                </span>
                <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
            </li>
        </transition-group>
    </section>
</template>

<script>
    export default {
        props: ["todoList"],
        methods: {
            toggleSuccess(todoItem, index) {
                this.$emit("toggleSuccess", todoItem, index);
            },
            editTodo(todoItem, index) {
                this.$emit("editTodo", todoItem, index);
            },
            removeTodo(todoItem, index) {
                this.$emit("removeTodo", todoItem, index);
            }
        }
    };
</script>

<style scoped>
    ul {
        list-style-type: none;
        padding-left: 0;
        margin-top: 0;
        text-align: left;
    }

    li {
        display: flex;
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        background: white;
        border-radius: 5px;
    }

    .checkBtn {
        line-height: 45px;
        color: #62acde;
        margin-right: 10px;
    }

    .expired {
        margin-left: auto;
        color: #de4343;
    }

    .editBtn {
        margin-left: 10px;
        color: #4caf50;
        cursor: pointer;
    }

    .removeBtn {
        margin-left: 10px;
        color: #de4343;
        cursor: pointer;
    }

    .list-item {
        display: inline-block;
        margin-right: 10px;
    }

    .list-move {
        transition: transform 1s;
    }

    .list-enter-active,
    .list-leave-active {
        transition: all 1s;
    }

    .list-enter,
    .list-leave-to {
        opacity: 0;
        transform: translateY(30px);
    }

    /* Tooltip container */
    #tooltip {
        position: relative;
        display: inline-block;
    }

    /* Tooltip text */
    #tooltip > .tooltip-text {
        visibility: hidden;
        width: 120px;
        background-color: black;
        color: #fff;
        text-align: center;
        border-radius: 6px;
        padding: 5px 0;
        position: absolute;
        z-index: 1;
        bottom: 150%;
        left: 50%;
        margin-left: -60px;
    }

    #tooltip > .tooltip-text::after {
        content: "";
        position: absolute;
        top: 100%;
        left: 50%;
        margin-left: -5px;
        border-width: 5px;
        border-style: solid;
        border-color: black transparent transparent transparent;
    }

    /* Show the tooltip text when you mouse over the tooltip container */
    #tooltip:hover > .tooltip-text {
        visibility: visible;
    }
</style>
