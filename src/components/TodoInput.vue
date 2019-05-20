<template>
    <div class="inputBox shadow">
        <span class="addBtn" v-on:click="addTodo">
            <i class="addBtnIcon fa fa-plus" aria-hidden="true"></i>
        </span>

        <div class="innerInputBox">
            <mdb-input class="title" label="title" size="lg" v-model="newTodoItemTitle" placehoder="title"
                       v-on:keyup.enter="addTodo" ref="title"/>
        </div>
        <div class="innerInputBox">
            <mdb-input type="textarea" label="content" :rows="2" class="content" v-model="newTodoItemContent"
                       v-on:keyup.enter="addTodo"/>
        </div>
        <div class="innerInputBox">
            <mdb-numeric-input class="priority" :min="0" :max="10" :emptyValue="newTodoItemPriority"
                               v-model="newTodoItemPriority" v-on:input="checkPriority" placeholder="priority"
                               v-on:keyup.enter="addTodo"
            />
        </div>
        <div class="innerInputBox">
            <i class="far fa-clock"></i>
            <input
                type="datetime-local"
                v-model="newTodoItemDeadline"
                placeholder="deadline"
                v-on:keyup.enter="addTodo"
                ref="deadline"
            />
        </div>
    </div>
</template>

<script>
    import {mdbInput, mdbNumericInput} from 'mdbvue';

    export default {
        data() {
            return {
                newTodoItemTitle: "",
                newTodoItemContent: "",
                newTodoItemDeadline: "",
                newTodoItemPriority: 0,
                showModal: false
            };
        },
        mounted() {
            // this.$refs.deadline.value = this.getDatetimeLocal();
            this.newTodoItemDeadline = this.getDatetimeLocal();
        },
        methods: {
            addTodo() {
                if (
                    this.newTodoItemTitle !== "" &&
                    this.newTodoItemContent !== ""
                ) {
                    let todo = {
                        title: this.newTodoItemTitle && this.newTodoItemTitle.trim(),
                        content: this.newTodoItemContent && this.newTodoItemContent.trim(),
                        success: false,
                        deadline: this.newTodoItemDeadline && this.newTodoItemDeadline.trim(),
                        priority: this.newTodoItemPriority || 0,
                    };
                    this.$emit("addTodo", todo);
                    this.clearInput();
                } else {
                    alert("please fill title and content");
                }
            },
            clearInput() {
                this.newTodoItemTitle = "";
                this.newTodoItemContent = "";
                this.newTodoItemPriority = 0;
                this.newTodoItemDeadline = this.getDatetimeLocal();
                this.$refs.title.focus();
            },
            checkPriority() {
                if (this.newTodoItemPriority < 0) this.newTodoItemPriority = 0;
                else if (this.newTodoItemPriority > 10)
                    this.newTodoItemPriority = 10;
                else if (this.newTodoItemPriority === "")
                    this.newTodoItemPriority = 0;
            },
            addZero(number) {
                if (0 < number && number < 10) {
                    return `0${number.toString()}`;
                } else {
                    return number;
                }
            },
            getDatetimeLocal() {
                const d = new Date();
                return `${d.getFullYear()}-${this.addZero(
                    d.getMonth()
                )}-${this.addZero(d.getDate())}T${this.addZero(
                    d.getHours()
                )}:${this.addZero(d.getMinutes())}`;
            }
        },
        components: {
            mdbInput: mdbInput,
            mdbNumericInput: mdbNumericInput,
        }
    };
</script>

<style scoped>
    .inputBox {
        background: white;
        height: fit-content;
        /*line-height: 50px;*/
        border-radius: 5px;
        text-align: center;
    }

    .innerInputBox {
        display: flex;
        justify-content: center;
        /*text-align: center;*/
        height: fit-content;
    }

    .innerInputBox > input,
    .innerInputBox > textarea,
    .innerInputBox > i {
        align-self: center;
        border-style: none;
        margin: 0 5px 0 5px;
    }

    .innerInputBox > .title,
    .innerInputBox > .content,
    .innerInputBox > .priority {
        width: 300px;
        margin: 5px 0 5px 0;
    }

    .addBtn {
        float: right;
        background: linear-gradient(to right, #6478fb, #8763fb);
        width: 3rem;
        height: 264.97px;
        border-radius: 0 5px 5px 0;
        cursor: pointer;
    }

    .addBtnIcon {
        color: white;
        margin: 50px 0;
    }

</style>
