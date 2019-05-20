<template>
    <div id="app">
        <TodoHeader></TodoHeader>
        <TodoInput v-on:addTodo="addTodo"></TodoInput>
        <TodoList v-bind:todoList="todoItems" v-on:removeTodo="removeTodo" v-on:toggleSuccess="toggleSuccess"
                  v-on:editTodo="editTodo"></TodoList>
        <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
        <!--<modal v-if="showModal" @save="saveEditedTodo" @close="closeEditedTodo">
            <div slot="header">
                <label>
                    <input type="text" class="title" v-model="modalTodo.title"/>
                </label>
            </div>

            <div slot="body">
                <label>
                    <textarea type="text" class="content" rows="3" cols="40" v-model="modalTodo.content"></textarea>
                </label>
                <label>
                    <input class="priority" type="number" min="0" max="10" v-model="modalTodo.priority"
                           v-on:input="checkPriority" placeholder="priority"/>
                </label>
            </div>
            <div slot="footer">
                <label>
                    <input type="datetime-local" v-model="modalTodo.deadline"/>
                </label>
            </div>
        </modal>-->
        <mdb-modal :show="showModal" @close="closeEditedTodo">
            <mdb-modal-header>
                <mdb-modal-title>
                    Edit Todo
                </mdb-modal-title>
            </mdb-modal-header>
            <mdb-modal-body>
                <mdb-input class="title" label="title" placehoder="title" v-model="modalTodo.title"></mdb-input>
                <mdb-input class="content" type="textarea" label="content" :rows="2" v-model="modalTodo.content"/>
                <mdb-numeric-input class="priority" :min="0" :max="10" default v-model="modalTodo.priority"
                                   v-on:input="checkPriority" placeholder="priority"/>
                <input type="datetime-local" class="datetime-local" v-model="modalTodo.deadline"/>
            </mdb-modal-body>
            <mdb-modal-footer>
                <mdb-btn class="save" color="primary" @click="saveEditedTodo">Save changes</mdb-btn>
            </mdb-modal-footer>
        </mdb-modal>
    </div>
</template>

<script>
    import TodoHeader from "./components/TodoHeader.vue";
    import TodoInput from "./components/TodoInput.vue";
    import TodoList from "./components/TodoList.vue";
    import TodoFooter from "./components/TodoFooter.vue";
    import Modal from "./components/common/Modal.vue";
    import {
        mdbBtn,
        mdbInput,
        mdbModal,
        mdbModalBody,
        mdbModalFooter,
        mdbModalHeader,
        mdbModalTitle,
        mdbNumericInput
    } from 'mdbvue';

    export default {
        name: 'App',
        data() {
            return {
                todoItems: [],
                showModal: false,
                editTodoIndex: undefined,
                modalTodo: {
                    title: "",
                    content: "",
                    priority: 0,
                    deadline: ""
                }
            };
        },
        created() {
            if (localStorage.length > 0) {
                // remove unnecessary data
                this.removeWebpackData();

                // get data from localStorage
                for (let i = 0; i < localStorage.length; i++) {
                    const todoKey = localStorage.key(i);

                    if (todoKey !== "loglevel:webpack-dev-server") {
                        this.todoItems.push(
                            JSON.parse(localStorage.getItem(todoKey))
                        );
                    }
                }
            }
        },
        mounted() {
            this.checkDeadline();
        },
        methods: {
            addTodo(todoItem) {
                localStorage.setItem(todoItem.title, JSON.stringify(todoItem));
                this.todoItems.push(todoItem);
            },
            clearAll() {
                localStorage.clear();
                this.todoItems = [];
            },
            removeTodo(todoItem, index) {
                localStorage.removeItem(index.toString());
                this.todoItems.splice(index, 1);
            },
            toggleSuccess(todoItem, index) {
                this.todoItems[index].success = !this.todoItems[index].success;
                localStorage.removeItem(index.toString());
                localStorage.setItem(todoItem.title, JSON.stringify(todoItem));
            },
            editTodo(todoItem, index) {
                // 수정 전 내용 모달 창 띄우기
                this.showModal = true;
                this.editTodoIndex = index;
                this.modalTodo.title = todoItem.title;
                this.modalTodo.content = todoItem.content;
                this.modalTodo.priority = todoItem.priority;
                this.modalTodo.deadline = todoItem.deadline;
            },
            closeEditedTodo() {
                // 모달 창 끄기
                this.showModal = false;
            },
            saveEditedTodo() {
                // 모달 창 끄기
                this.showModal = false;

                // 수정된 내역 저장하기.
                if (this.editTodoIndex !== undefined) {
                    this.todoItems[this.editTodoIndex].title = this.modalTodo.title;
                    this.todoItems[this.editTodoIndex].content = this.modalTodo.content;
                    this.todoItems[this.editTodoIndex].priority = this.modalTodo.priority;
                    this.todoItems[this.editTodoIndex].deadline = this.modalTodo.deadline;

                    // remove unnecessary data in localStorage
                    this.removeWebpackData();

                    // update todoItem in localStorage
                    let todoItem = JSON.parse(
                        localStorage.getItem(
                            localStorage.key(this.editTodoIndex.toString())
                        )
                    );
                    localStorage.removeItem(
                        localStorage.key(this.editTodoIndex.toString())
                    );
                    todoItem.title = this.modalTodo.title;
                    todoItem.content = this.modalTodo.content;
                    todoItem.priority = this.modalTodo.priority;
                    todoItem.deadline = this.modalTodo.deadline;
                    localStorage.setItem(todoItem.title, JSON.stringify(todoItem));
                }
            },
            checkPriority() {
                if (this.modalTodo.priority < 0) this.modalTodo.priority = 0;
                else if (this.modalTodo.priority > 10) this.modalTodo.priority = 10;
                else if (this.modalTodo.priority === "")
                    this.modalTodo.priority = 0;
            },
            // remove unnecessary data at localStorage
            removeWebpackData() {
                for (let i = 0; i < localStorage.length; i++) {
                    const todoKey = localStorage.key(i);
                    if (todoKey === "loglevel:webpack-dev-server") {
                        localStorage.removeItem("loglevel:webpack-dev-server");
                        break;
                    }
                }
            },
            // 10의 자리수 0 채우는 함수
            addZero(number) {
                if (0 < number && number < 10) {
                    return `0${number.toString()}`;
                } else {
                    return number;
                }
            },
            checkDeadline() {
                // check if it was expired every 10s
                setInterval(() => {
                    if (this.todoItems.length > 0) {
                        const currentDate = new Date();
                        for (const todoItem of this.todoItems) {
                            // if deadline was set, check deadline
                            if (
                                todoItem.deadline !== "" &&
                                this.checkDueDeadLine(todoItem.deadline, currentDate) &&
                                todoItem.expired === undefined
                            ) {
                                // set attribute to be reactive.
                                // so, Vue observe this attribute, rerender if it modified.
                                this.$set(todoItem, "expired", true);
                            }
                        }
                    }
                }, 10000);
            },
            checkDueDeadLine(deadline, currentDate) {
                const deadlineDate = new Date(deadline);
                return deadlineDate.getTime() < currentDate.getTime();
            }
        },
        components: {
            TodoHeader: TodoHeader,
            TodoInput: TodoInput,
            TodoList: TodoList,
            TodoFooter: TodoFooter,
            Modal: Modal,
            mdbInput,
            mdbNumericInput,
            mdbModal,
            mdbModalHeader,
            mdbModalTitle,
            mdbModalBody,
            mdbModalFooter,
            mdbBtn
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 100px;
    }

    body {
        text-align: center;
        background-color: #f6f6f8;
    }

    .shadow {
        box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
    }

    .show-like-table {
        width: 200px;
    }

    .datetime-local {
        border-style: none;
    }

    .modal-title {
        font-size: 2rem;
    }
</style>
