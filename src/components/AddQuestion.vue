<template>
    <h3 class="text-capitalize">flashcards</h3>
    <button v-if="!showAddCard" @click.prevent="showAddCard = !showAddCard" class="btn text-capitalize my-2 show-btn"
        id="show-btn">add question</button>
    <div v-else class="card card-body my-3">
        <!-- close btn -->
        <a href="#" class="close-btn mt-0" @click.prevent="cancelNewNote">
            X
        </a>
        <!-- end of close btn -->
        <div v-show="showAlertMsg" class="alert alert-danger w-75 text-capitalize">
            You can't add blank fields
        </div>
        <form id="question-form" @submit.prevent="addNewNote">
            <!-- single input -->
            <h5 class="text-capitalize">question</h5>
            <div class="form-group">
                <textarea class="w-100" id="question-input" rows="3" v-model="noteQuestion"></textarea>
            </div>
            <!-- end of single input -->
            <!-- single input -->
            <h5 class="text-capitalize">answer</h5>
            <div class="form-group">
                <textarea class="w-100" id="answer-input" rows="3" v-model="noteAnswer"></textarea>
            </div>
            <!-- end of single input -->
            <button type="submit" class="btn submitBtn text-capitalize w-50">save</button>
        </form>
    </div>
</template>

<script>
export default {
    emits: ['newNote'],
    data() {
        return {
            showAddCard: false,
            showAlertMsg: false,
            noteQuestion: "",
            noteAnswer: ""
        }
    },
    methods: {
        addNewNote() {
            if (this.noteQuestion.trim() == "" || this.noteAnswer.trim() == "") {
                this.showAlertMsg = true;

                setTimeout(() => {
                    this.showAlertMsg = false;
                }, 2000);

                return;
            }

            this.$emit("newNote", { question: this.noteQuestion, answer: this.noteAnswer });
            this.showAddCard = !this.showAddCard;
            this.noteQuestion = this.noteAnswer = "";
        },
        cancelNewNote() {
            this.showAddCard = !this.showAddCard;
            this.noteQuestion = this.noteAnswer = "";
        }
    }
}
</script>

<style>
</style>