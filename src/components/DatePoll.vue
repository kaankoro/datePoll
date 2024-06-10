<template>
     <div class="flex-side w-full flex justify-start items-center px-4 absolute left-0 top-2 z-50">
        <span class="shadow-lg w-34px h-32px flex justify-center items-center rounded-full bg-dark opacity-70 cursor-pointer hover:bg-dark" @click="prevQuestion">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" class="ml--2px">
            <mask id="pt_activePageIndex" style="mask-type:alpha;" maskUnits="userSpaceOnUse" x="8" y="6" width="8" height="12">
              <path d="M15.0005 17.2892C15.3905 16.8992 15.3905 16.2692 15.0005 15.8792L11.1205 11.9992L15.0005 8.11925C15.3905 7.72925 15.3905 7.09925 15.0005 6.70925C14.6105 6.31925 13.9805 6.31925 13.5905 6.70925L9.00047 11.2992C8.61047 11.6892 8.61047 12.3192 9.00047 12.7092L13.5905 17.2992C13.9705 17.6792 14.6105 17.6792 15.0005 17.2892Z" fill="white"></path>
            </mask>
            <g mask="url(#pt_activePageIndex)">
              <rect x="24" y="24" width="24" height="24" transform="rotate(180 24 24)" fill="white"></rect>
            </g>
          </svg>
        </span>
        <div class="w-full items-start pl-2 flex-col h-44px py-1">
          <span id="question-number" class="text-sm text-[#C7CED1] flex justify-between items-center gap-1">{{ currentQuestionNumber }} <p class="text-sm">of</p> {{ totalQuestions }}</span>
        </div>
      </div>
      <div class="settings">
      <span class="fas fa-cog" @click="toggleSettings" style="width: 100px; height: 100px; font-size: 25px;">&#x2699;</span>
      <div v-if="showSettings" class="dropdown-menu">
        <label>
          <input type="radio" value="dd.mm.yyyy" v-model="userDateFormat" @change="resetDateFields">
          DD.MM.YYYY
        </label>
        <label>
          <input type="radio" value="mm.dd.yyyy" v-model="userDateFormat" @change="resetDateFields">
          MM.DD.YYYY
        </label>
        <label>
          <input type="radio" value="yyyy.mm.dd" v-model="userDateFormat" @change="resetDateFields">
          YYYY.MM.DD
        </label>
      </div>
    </div>
    <div style="position: absolute; top: 50%; left: 50%; transform: translateX(-50%) translateY(-50%);" class="flex h-full overflow-hidden justify-center items-center pb-60px interactive-container bg-white">
     
      <div class="flex flex-col w-full h-full items-center lg:p-4 p-0 px-4 justify-center pt-14">
        <div class="flex w-full flex-col items-center max-w-420px max-h-calc(100%-70px)">
          <h2 id="question-title" class="widget-title w-full break-words lg:text-2xl text-lg font-semibold text-gray-1 text-[#4D525D] text-start">{{ currentQuestion.questionTitle }}</h2>
          <div class="lg:h-auto widget-box flex flex-col w-full items-center overflow-hidden overflow-y-auto lg:pb-auto justify-normal">
            <p class="widget-desc w-full break-words lgtext-lg text-base text-gray-1.5 mt-2 text-[#4D525D] text-start">{{ currentQuestion.questionDescription }}</p>
            <div class="date-input w-full lg:pr-4 flex flex-col gap-1 lg:gap-2 lg:max-h-full max-h-300px overflow-hidden overflow-y-scroll mt-4">
    <div class="date-field">
    <label v-if="effectiveDateFormat === 'yyyy.mm.dd'" for="year-input" class="block text-sm font-medium text-gray-700">Year</label>
    <input v-if="effectiveDateFormat === 'yyyy.mm.dd'" :class="yearClass" id="year-input" placeholder="YYYY" v-model="year" @input="validateYear" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
    <label v-if="effectiveDateFormat === 'mm.dd.yyyy'" for="month-input" class="block text-sm font-medium text-gray-700">Month</label>
    <input v-if="effectiveDateFormat === 'mm.dd.yyyy'" :class="monthClass" id="month-input" placeholder="MM" v-model="month" @input="validateMonth" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
    <label v-if="effectiveDateFormat === 'dd.mm.yyyy'" for="day-input" class="block text-sm font-medium text-gray-700">Day</label>
    <input v-if="effectiveDateFormat === 'dd.mm.yyyy'" :class="dayClass" id="day-input" placeholder="DD" v-model="day" @input="validateDay" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
  </div>
<div class="separator">/</div>
<div class="date-field">

    <label v-if="effectiveDateFormat === 'yyyy.mm.dd'" for="month-input" class="block text-sm font-medium text-gray-700">Month</label>
    <input v-if="effectiveDateFormat === 'yyyy.mm.dd'" :class="monthClass" id="month-input" placeholder="MM" v-model="month" @input="validateMonth" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
    <label v-if="effectiveDateFormat === 'mm.dd.yyyy'" for="day-input" class="block text-sm font-medium text-gray-700">Day</label>
    <input v-if="effectiveDateFormat === 'mm.dd.yyyy'" :class="dayClass" id="day-input" placeholder="DD" v-model="day" @input="validateDay" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
    <label v-if="effectiveDateFormat === 'dd.mm.yyyy'" for="month-input" class="block text-sm font-medium text-gray-700">Month</label>
    <input v-if="effectiveDateFormat === 'dd.mm.yyyy'" :class="monthClass" id="month-input" placeholder="MM" v-model="month" @input="validateMonth" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
  </div>
<div class="separator">/</div>
    <div class="date-field">
    
    <label v-if="effectiveDateFormat === 'yyyy.mm.dd'" for="day-input" class="block text-sm font-medium text-gray-700">Day</label>
    <input v-if="effectiveDateFormat === 'yyyy.mm.dd'" :class="dayClass" id="day-input" placeholder="DD" v-model="day" @input="validateDay" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
    <label v-if="effectiveDateFormat === 'mm.dd.yyyy'" for="year-input" class="block text-sm font-medium text-gray-700">Year</label>
    <input v-if="effectiveDateFormat === 'mm.dd.yyyy'" :class="yearClass" id="year-input" placeholder="YYYY" v-model="year" @input="validateYear" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
    <label v-if="effectiveDateFormat === 'dd.mm.yyyy'" for="year-input" class="block text-sm font-medium text-gray-700">Year</label>
    <input v-if="effectiveDateFormat === 'dd.mm.yyyy'" :class="yearClass" id="year-input" placeholder="YYYY" v-model="year" @input="validateYear" class="input-field-empty mt-1 block w-1/3 px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
  </div>
    
  </div>
  <button @click="submitDate" class="submit-button rounded-md">Submit</button>

          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        questions: [],
        currentQuestionIndex: 0,
        month: '',
        userDateFormat: '',
      day: '',
      year: '',
      dateFormat: '',
      showSettings: false,
      };
    },
    computed: {
      effectiveDateFormat() {
      return this.userDateFormat || this.dateFormat;
    },
      currentQuestion() {
        return this.questions[this.currentQuestionIndex] || {};
      },
      currentQuestionNumber() {
        return this.currentQuestionIndex + 1;
      },
      totalQuestions() {
        return this.questions.length;
      },
      yearClass() {
    return this.year ? 'input-field' : 'input-field-empty';
  },
  monthClass() {
    return this.month ? 'input-field' : 'input-field-empty';
  },
  dayClass() {
    return this.day ? 'input-field' : 'input-field-empty';
  }
    },
    mounted() {
      this.fetchQuestionData(1);
    },

    methods: {
      toggleSettings() {
      this.showSettings = !this.showSettings;
    },
        resetDateFields() {
    this.year = '';
    this.month = '';
    this.day = '';
  },

        async fetchQuestionData(questionNumber) {
  try {
    const response = await fetch('questions-data.json');
    this.questions = await response.json();
    const question = this.questions.find(q => q.questionNumber === questionNumber);
    if (question) {
      this.dateFormat = question.dateFormat;
      this.resetDateFields();
    } else {
      console.error('Question not found with number:', questionNumber);
    }
  } catch (error) {
    console.error('Error fetching the question data:', error);
  }
},

validateMonth() {
    const month = parseInt(this.month, 10);
    if (!/^\d{0,2}$/.test(this.month) || month < 0 || month > 12) {
      this.month = '';
    }
  },
  validateDay() {
    const day = parseInt(this.day, 10);
    if (!/^\d{0,2}$/.test(this.day) || day < 0 || day > 31) {
      this.day = '';
    }
  },
  validateYear() {
    const year = parseInt(this.year, 10);
    if (!/^\d{0,4}$/.test(this.year)) {
      this.year = '';
    }
  },
    isValidDate() {
      const month = parseInt(this.month);
      const day = parseInt(this.day);
      const year = parseInt(this.year);
      const date = new Date(year, month - 1, day);
      return (
        date.getMonth() === month - 1 &&
        date.getDate() === day &&
        date.getFullYear() === year
      );
    },

      submitDate() {
        if (!this.isValidDate()) {
        console.log('Please enter a valid date.');
        return;
      }
    const month = parseInt(document.getElementById('month-input').value);
    const day = parseInt(document.getElementById('day-input').value);
    const year = parseInt(document.getElementById('year-input').value);


    const date = new Date(year, month - 1, day); 
    const currentDate = new Date();

    if (date > currentDate) {
        console.log('The selected date cannot be in the future.');
        return;
    }

    if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++;
        document.getElementById('month-input').value = '';
        document.getElementById('day-input').value = '';
        document.getElementById('year-input').value = '';
        this.fetchQuestionData(this.currentQuestionNumber);

    } else {
        console.log('Survey completed.');
    }

    console.log(`Question ${this.currentQuestionNumber}: User selected date:`, date);
}

,
      prevQuestion() {
        if (this.currentQuestionIndex > 0) {
          this.currentQuestionIndex--;
          this.userDate = '';
          this.fetchQuestionData(this.currentQuestionNumber);
        }
      }
    }
  };
  </script>
  
  <style scoped>

  .flex { display: grid; }
  #question-number {display: ruby;}
  .date-input {
    display: flex;     
    -webkit-box-align: baseline;
    align-items: baseline;
    width: fit-content;
}
  .h-full { height: 100%; }
  .overflow-hidden { overflow: hidden; }
  .justify-center { justify-content: center; }
  .items-center { align-items: center; }
  .pb-60px { padding-bottom: 60px; }
  .interactive-container { position: relative; }
  .bg-white { background-color: white; }
  .absolute { position: absolute; }
  .left-0 { left: 0; }
  .top-2 { top: 0.5rem; }
  .z-50 { z-index: 50; }
  .shadow-lg { --tw-shadow: 0 10px 15px -3px rgb(0 0 0 / .1), 0 4px 6px -4px rgb(0 0 0 / .1) !important;
    --tw-shadow-colored: 0 10px 15px -3px var(--tw-shadow-color), 0 4px 6px -4px var(--tw-shadow-color) !important;
    box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow) !important; }
  .rounded-full { border-radius: 9999px !important; }
  .bg-dark {     --tw-bg-opacity: 1 !important;
    background-color: rgb(17 24 39 / 1) !important;}
  .opacity-70 { opacity: 0.7; }
  .cursor-pointer { cursor: pointer; }
  .hover\:bg-dark:hover { background-color: #111827; }
  .ml--2px { margin-left: -2px; }
  .text-sm { font-size: 0.875rem; }
  .widget-title { font-size: 1.5rem !important;
        line-height: 2rem !important;
       color: rgb(17 24 39  / 1)!important;
    font-weight: 600;
text-align: center;
width: 100%;
font-family: Urbanist, ui-sans-serif, system-ui, sans-serif, 
"Apple Color Emoji", "Segoe UI Emoji", Segoe UI Symbol, "Noto Color Emoji" !important;

}
  .widget-desc { font-size: 1rem; 
    color: rgb(77 82 93 / 1) !important;
    font-family: Urbanist, ui-sans-serif, system-ui, sans-serif, 
"Apple Color Emoji", "Segoe UI Emoji", Segoe UI Symbol, "Noto Color Emoji" !important;
font-weight: 400;
font-size: 1rem !important;
    line-height: 1.5rem !important;
    text-align: center !important;}
  .text-base { font-size: 1rem; }
  .lg\:text-lg { font-size: 1.125rem; }
  .mt-4 { margin-top: 1rem; }
  .gap-1 { gap: 0.25rem; }
  .gap-2 { gap: 0.5rem; }
  .overflow-y-scroll { overflow-y: scroll; }
  .lg\:max-h-full { max-height: 100%; }
  .block { display: block; }
  .mt-1 { margin-top: 0.25rem; }
  .px-3 { padding-left: 0.75rem; padding-right: 0.75rem; }
  .py-2 { padding-top: 0.5rem; padding-bottom: 0.5rem; }
  .border { border-width: 1px; }
  .border-gray-300 { border-color: #D1D5DB; }
  .rounded-md { border-radius: 0.375rem; }
  .shadow-sm { box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05); }
  .focus\:outline-none:focus { outline: 2px solid transparent; outline-offset: 2px; }
  .focus\:border-indigo-500:focus { border-color: #6366F1; }
  .sm\:text-sm { font-size: 0.875rem; }
  .mt-4 { margin-top: 1rem; }
  .bg-blue-500 { background-color: #3B82F6; }
  .text-white { color: white; }
  .submit-button{
    background-color: rgb(17, 24, 39);
    color: rgb(255, 255, 255);
    font-size: 1.25rem !important;
        line-height: 1.75rem !important;
        font-weight: 400 !important;
        padding-top: .5rem !important;
    padding-bottom: .5rem !important;
    max-width: 420px !important;
    margin-top: 10px;
    border: none;
  }
  .submit-button:hover{
    color: #888c94;
  }
  .date-field {
    font-family: Urbanist, ui-sans-serif, system-ui, sans-serif, 
"Apple Color Emoji", "Segoe UI Emoji", Segoe UI Symbol, "Noto Color Emoji";
  }
  .separator {
    margin-top: auto;
    margin-right: 12px;
    max-width: 100%;
    width: inherit;
    font-weight: unset;
    font-size: 30px;
    line-height: 38px;
    color: rgb(77 82 93 / 1);
  }
  .input-field-empty {
    display: block;
    width: 100%;
    font-family: inherit;
    color: rgb(77 82 93 / 1);
    padding-block-start: 0px;
    padding-block-end: var(--spacing-100);
    padding-inline: 0px;
    border: none;
    outline: none;
    border-radius: 0px;
    appearance: none;
    background-image: none;
    background-position: initial;
    background-size: initial;
    background-repeat: initial;
    background-attachment: initial;
    background-origin: initial;
    background-clip: initial;
    transform: translateZ(0px);
    font-size: 30px;
    -webkit-font-smoothing: antialiased;
    line-height: unset;
    -webkit-text-fill-color: rgb(180, 199, 231);
    animation: 1ms ease 0s 1 normal none running native-autofill-in;
    transition: background-color 1e+08s ease 0s, box-shadow 0.1s ease-out 0s;
    box-shadow: rgba(4, 69, 175, 0.3) 0px 1px;
    background-color: transparent !important;
  }
  .input-field {
    display: block;
    width: 100%;
    font-family: inherit;
    color: rgb(77 82 93 / 1);
    padding-block-start: 0px;
    padding-block-end: var(--spacing-100);
    padding-inline: 0px;
    border: none;
    outline: none;
    border-radius: 0px;
    appearance: none;
    background-image: none;
    background-position: initial;
    background-size: initial;
    background-repeat: initial;
    background-attachment: initial;
    background-origin: initial;
    background-clip: initial;
    transform: translateZ(0px);
    font-size: 30px;
    -webkit-font-smoothing: antialiased;
    line-height: unset;
    -webkit-text-fill-color: rgb(0, 16, 41);
    animation: 1ms ease 0s 1 normal none running native-autofill-in;
    transition: background-color 1e+08s ease 0s, box-shadow 0.1s ease-out 0s;
    box-shadow: rgba(4, 69, 175, 0.3) 0px 1px;
    background-color: transparent !important;
  }
  #day-input, #month-input {
    width: calc(4ch);
  }
  #year-input {
    width: calc(6ch);
  }
  .w-34px{
    width: 34px !important;
  } 
  .h-32px {
    height: 32px !important;
  }
  .flex-side {
    display: flex;
    margin-left: 15px;
  }
  #question-number {
    margin:10px;
    --tw-text-opacity: 1 !important;
    color: rgb(199 206 209 / 1) !important;
    font-family: Urbanist, ui-sans-serif, system-ui, sans-serif, 
"Apple Color Emoji", "Segoe UI Emoji", Segoe UI Symbol, "Noto Color Emoji" !important;
font-weight: 100;
  }
.settings {
  position: relative;
  display: inline-block;
  left:98%;
}

.fas.fa-cog {
  cursor: pointer;
}

.dropdown-menu {
  display: block;
  position: absolute;
  left: auto;
  right: 0;
  top: 20px;
  background-color: white;
  border: 1px solid #ccc;
  padding: 10px;
  z-index: 1000;
}
  </style>
  