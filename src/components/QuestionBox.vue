<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in shuffledAnswers"
                    :key="index"
                    @click.prevent="selectAnswer(index)"
                    :class="getAnswerClasses(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                確定
            </b-button>

            <b-button @click="next" variant="success" href="#">下一題</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        };
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index;
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];

            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        submitAnswer() {
            let isCorrect = (this.selectedIndex === this.correctIndex) ? true : false;
            console.log(this.selectedIndex, this.correctIndex, isCorrect);

            this.increment(isCorrect);

            this.answered = true;
        },
        getAnswerClasses(index) {
            let classes = [];

            if(!this.answered && index === this.selectedIndex) {
                classes.push('selected');
            }

            if(this.answered && index === this.correctIndex) {
                classes.push('correct');
            }

            if(this.answered && index === this.selectedIndex && index !== this.correctIndex) {
                classes.push('incorrect');
            }
            
            return classes;
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.shuffleAnswers();
                this.answered = false;
            }
        }
    }
}
</script>

<style scoped>
    .list-group {
        margin: 15px;
    }

    .list-group-item:hover {
        background-color: #EEE;
        cursor: pointer;
    }

    .selected {
        background-color: lightblue;
    }

    .correct {
        background-color: lightgreen;
    }

    .incorrect {
        background-color: red;
    }

    .btn {
        margin: 0 5px;
    }
</style>