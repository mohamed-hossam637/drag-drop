<template>
  <header class="container py-3">
    <div></div>
    <div class="score fw-bold d-flex">
      <div class="icon"><i class="fa-solid fa-star"></i></div>
      <div>
        {{ score }} / {{ totalAnswers }}
      </div>
    </div>
  </header>
  <div class="container py-5">
      <div class="row py-5 ">
        <div class=" col-11 board">
          <p v-for="(question, index) in questions" :key="question.id">
            {{ replace(question.title)[0] }}
            <span class="separate" :id="question.id" :data-index="index" @dragover.prevent @drop="onDrop"> {{ question.answer }} </span>
            {{ replace(question.title)[1] }}
          </p>
        </div>
      </div>
    <ul class="answers">
      <li :ref="answer.title" class="answer" draggable="true" @dragstart="onDrag(index)" v-for="(answer, index) in answers" v-bind:key="answer.id" :id='answer.id'> {{ answer.title }}</li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'HomeView',
  components: {},
  data () {
    return {
      questions: [
        { title: 'This weekend, iam going to @A Washington,', answer: '......', id: 1 },
        { title: 'D.C. it\'s the capital of the @A !', answer: '......', id: 2 },
        { title: 'i\'m @A visit The White House, ', answer: '......', id: 3 },
        { title: 'which is the @A house. ', answer: '......', id: 4 },
        { title: 'There are 132 @A in it. ', answer: '......', id: 5 },
        { title: 'Can you imagine? One room is the @A .', answer: '......', id: 6 },
        { title: 'The President @A there. it\'s going to be fun!', answer: '......', id: 7 }
      ],
      answers: [
        { title: 'going to', id: 1, question_id: 3 },
        { title: 'Oval Office', id: 2, question_id: 6 },
        { title: 'visit', id: 3, question_id: 1 },
        { title: 'United States', id: 4, question_id: 2 },
        { title: 'rooms', id: 5, question_id: 5 },
        { title: 'works', id: 6, question_id: 7 },
        { title: 'Presidents', id: 7, question_id: 4 }
      ],
      draggedItem: null,
      wrong: false,
      score: 0
    }
  },
  methods: {
    onDrag (index) {
      this.draggedItem = this.answers[index]
    },
    onDrop (e) {
      const answerQuestionId = this.draggedItem.question_id
      const questionId = +e.target.getAttribute('id')
      const questionIndex = e.target.getAttribute('data-index')
      const answerElementId = this.draggedItem.title
      // check if the answer is right or wrong
      // if answer is right remove answer from answers object and update answer property in question
      if (answerQuestionId === questionId) {
        e.target.classList.add('underline')
        e.target.classList.add('right')
        this.score += 1
        this.questions[questionIndex].answer = this.draggedItem.title
        this.answers = this.answers.filter((ele) => {
          return ele.id !== this.draggedItem.id
        })
      } else {
        this.$refs[this.draggedItem.title][0].classList.add('wrong')
        setTimeout(() => {
          this.$refs[this.draggedItem.title][0].classList.remove('wrong')
        }, 1000);
      }
    },
    replace (q) {
      const strSplit = q.split('@A')
      return strSplit
    }
  },
  computed:{
    totalAnswers() {
      return this.questions.length
    }
  }
}
</script>
<style>
header {
  display: flex;
  justify-content: space-between;
}
.score {
  border-radius: 50px;
  align-items: center;
  border: 4px solid #fff;
  padding: 0 15px 0 0;
  background-color: #0c2461;
  color: #fff;
}
.score .icon {
  margin: 0;
  margin-right: 20px;
  padding: 5px 10px;
  border: 3px solid #fff;
  border-radius: 50px;
  color: #FFD541;
}
.row {
  justify-content: center;
}
.board {
  background-color: #59CBE8;
  padding: 3em 2em;
  border-radius: 10px;
  border: 5px solid #fff;
}
p{
  font-size: 25px;
  line-height: 1.5;
  /* margin-bottom: 20px; */
  display: inline-block;
}
.separate {
  background-color: transparent;
  padding: 0 15px;
  margin-bottom: 20px;
  transform: scale3d(1,1);
  display: inline-block;
  text-align: center;
}
.separate.underline {
  color: #2980b9;
  text-decoration: underline;
  text-decoration-style: dotted;
  font-weight: bold;

}
ul{
  list-style: none;
  display: flex;
  justify-content: center;
}
.answer {
  cursor: move;
  padding: 8px 20px;
  margin-right: 10px;
  background-color: #eee;
  color:#333;
  display: inline-block;
  border-radius: 50px;
  font-size: 1.2em;
  font-weight: bold;
}
@keyframes myAnim {
	0% {
		transform: scale3d(1, 1, 1);
	}

	30% {
		transform: scale3d(1.25, 0.75, 1);
	}

	40% {
		transform: scale3d(0.75, 1.25, 1);
	}

	50% {
		transform: scale3d(1.15, 0.85, 1);
	}

	65% {
		transform: scale3d(0.95, 1.05, 1);
	}

	75% {
		transform: scale3d(1.05, 0.95, 1);
	}

	100% {
		transform: scale3d(1, 1, 1);
	}
}
.right {
  animation: myAnim 2s ease 0s 1 normal forwards;
}
@keyframes myAnim2 {
	0%,
	100% {
		transform: translateX(0%);
		transform-origin: 50% 50%;
	}

	15% {
		transform: translateX(-30px) rotate(-6deg);
	}

	30% {
		transform: translateX(15px) rotate(6deg);
	}

	45% {
		transform: translateX(-15px) rotate(-3.6deg);
	}

	60% {
		transform: translateX(9px) rotate(2.4deg);
	}

	75% {
		transform: translateX(-6px) rotate(-1.2deg);
	}
}
.wrong {
  animation: myAnim2 2s ease 0s 1 normal forwards;
}
</style>
