<template>
    <div class="col-md-4">
        <div v-if="showEditCard" class="card card-body my-3">
            <!-- end of close btn -->
            <div class="feedback alert w-75 text-capitalize">
                customer feedback
            </div>
            <form id="question-form" @submit.prevent>
                <!-- single input -->
                <h5 class="text-capitalize">question</h5>
                <div class="form-group">
                    <textarea class="w-100" id="question-input" rows="3" v-model="noteQuestionEdited"></textarea>
                </div>
                <!-- end of single input -->
                <!-- single input -->
                <h5 class="text-capitalize">answer</h5>
                <div class="form-group">
                    <textarea class="w-100" id="answer-input" rows="3" v-model="noteAnswerEdited"></textarea>
                </div>
                <!-- end of single input -->
                <button @click.prevent="submitEditNote" type="submit"
                    class="btn submitBtn text-capitalize w-50">save</button>
                <button @click.prevent="cancelEditNote" type="submit"
                    class="btn cancelBtn text-capitalize w-50">Cancel</button>
            </form>
        </div>

        <div v-else class="card card-body flashcard my-3">
            <h4 class="text-capitalize" v-html="noteQuestion"></h4>
            <a href="#" class="text-capitalize my-3 show-answer" @click="showAnswer = !showAnswer">show/hide answer</a>
            <h5 v-if="showAnswer" class="answer mb-3">{{ noteAnswer }}</h5>
            <div class="flashcard-btn d-flex justify-content-between">
                <a href="#" @click.prevent="showEditCard = !showEditCard" id="edit-flashcard"
                    class=" btn my-1 edit-flashcard text-uppercase" data-id="">edit</a>
                <a href="#" @click.prevent="deleteCard" id="delete-flashcard"
                    class=" btn my-1 delete-flashcard text-uppercase">delete</a>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    props: {
        note: Object
    },
    emits: ['delete', 'edit'],
    data(props) {
        return {
            showEditCard: false,
            showAnswer: false,
            noteId: props.note.id,
            noteQuestion: props.note.question,
            noteAnswer: props.note.answer,
            noteQuestionEdited: props.note.question,
            noteAnswerEdited: props.note.answer
        }
    },
    methods: {
        submitEditNote() {
            this.noteQuestion = this.noteQuestionEdited;
            this.noteAnswer = this.noteAnswerEdited;
            this.showEditCard = !this.showEditCard;

            this.$emit("edit", { id: this.noteId, newData: { question: this.noteQuestionEdited, answer: this.noteAnswerEdited } });
        },
        cancelEditNote() {
            this.noteQuestionEdited = this.noteQuestion;
            this.noteAnswerEdited = this.noteAnswer;
            this.showEditCard = !this.showEditCard;
        },
        deleteCard() {
            this.$emit("delete", this.noteId);
        }
    }
}
</script>

<style>
</style>