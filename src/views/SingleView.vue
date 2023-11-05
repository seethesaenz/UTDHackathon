<script setup>
import { ref, computed } from 'vue'
import Button from '../components/Button.vue'
import ConfettiExplosion from "vue-confetti-explosion";

const monthlyCarPayment = ref(0)
const studentLoanPayment = ref(0)
const monthlyMortgagePayment = ref(0)
const creditScore = ref(0)
const grossMonthlyIncome = ref(0)
const monthlyCreditCardPayment = ref(0)
const homeAppraisedValue = ref(0)
const downPaymentAmount = ref(0)
const loanAmount = ref(0)
const showCalculations = ref(false)

// computed properties
const ltv = computed(() => {
    return loanAmount.value / homeAppraisedValue.value
})

const dti = computed(() => {
    return (monthlyCarPayment.value + studentLoanPayment.value + monthlyMortgagePayment.value + monthlyCreditCardPayment.value) / grossMonthlyIncome.value
})

const fedti = computed(() => {
    return (monthlyMortgagePayment.value) / (grossMonthlyIncome.value)
})

const ltvAcceptable = computed(() => {
    return ltv.value <= 95
})

const dtiAcceptable = computed(() => {
    return dti.value <= 0.36
})

const fedtiAcceptable = computed(() => {
    return fedti.value <= 0.28
})

const creditScoreAcceptable = computed(() => {
    return creditScore.value >= 640
})

const approved = computed(() => {
    return ltvAcceptable.value && dtiAcceptable.value && fedtiAcceptable.value && creditScoreAcceptable.value
})

function handleSubmit() {
    showCalculations.value = true
}

</script>

<template>
    <div class="container">
        <h1>Loan Approval Calculator</h1>

        <div class="input-group">
            <label>Monthly Car Payment</label>
            <input v-model="monthlyCarPayment" type="number" placeholder="Monthly Car Payment" min="0" step="1" />
        </div>


        <div class="input-group">
            <label>Student Loan Payment</label>
            <input v-model="studentLoanPayment" type="number" placeholder="Student Loan Payment" min="0" />
        </div>

        <div class="input-group">
            <label>Monthly Mortgage Payment</label>
            <input v-model="monthlyMortgagePayment" type="number" placeholder="Monthly Mortgage Payment" min="0" />
        </div>

        <div class="input-group">
            <label>Credit Score</label>
            <input v-model="creditScore" type="number" placeholder="Credit Score" min="0" />
        </div>

        <div class="input-group">
            <label>Gross Monthly Income</label>
            <input v-model="grossMonthlyIncome" type="number" placeholder="Gross Monthly Income" min="0" />
        </div>

        <div class="input-group">
            <label>Monthly Credit Card Payment</label>
            <input v-model="monthlyCreditCardPayment" type="number" placeholder="Monthly Credit Card Payment" min="0" />
        </div>

        <div class="input-group">
            <label>Home Appraised Value</label>
            <input v-model="homeAppraisedValue" type="number" placeholder="Home Appraised Value" min="0" />
        </div>

        <div class="input-group">
            <label>Down Payment Amount</label>
            <input v-model="downPaymentAmount" type="number" placeholder="Down Payment Amount" min="0" />
        </div>

        <div class="input-group">
            <label>Loan Amount</label>
            <input v-model="loanAmount" type="number" placeholder="Loan Amount" min="0" />
        </div>

        <Button id="BIGBITTON" style="margin-top: 10px" v-show="!showCalculations" :onClick="handleSubmit">Calculate</Button>

        
        <div v-show="showCalculations">
            <p class="approved" v-if="approved">You are approved!</p>
            <p class="not-approved" v-else>You are not approved.</p>

            <p class="error" v-if="!ltvAcceptable">Your <a href="https://singlefamily.fanniemae.com/originating-underwriting/mortgage-products/97-loan-value-options">LTV</a> is too high.</p>
            <p class="error" v-if="!dtiAcceptable">Your <a href="https://yourhome.fanniemae.com/buy/why-understanding-debt-essential">DTI</a> is too high.</p>
            <p class="error" v-if="!fedtiAcceptable">Your <a href="https://www.investopedia.com/terms/f/front-end-debt-to-income-ratio.asp#:~:text=What%20Is%20Front%2DEnd%20Debt,2">Front End DTI</a> is too high.</p>
            <p class="error" v-if="!creditScoreAcceptable">Your <a href="https://www.investopedia.com/terms/c/credit_score.asp">credit score</a> is too low.</p>
        </div>

        <ConfettiExplosion style="position: fixed; top: 50%; left: 50%" v-if="showCalculations && approved" />

        <!-- <div style="font-weight: bold;">
            <p>Loan to Value: {{ ltv }}</p>
            <p>Debt to Income: {{ dti }}</p>
            <p>Front End Debt to Income: {{ fedti }}</p>
        </div> -->
    </div>
</template>

<style scoped>
.not-approved {
    color : #721c24;
    font-weight: bold;
    text-align: center;
    font-size: 25px;
    margin-top: 10px;
    background-color: #f8d7da;
    padding: 10px;
    border-radius: 5px;
    border: 2px solid #f5c6cb;
}

.approved {
    color : #155724;
    font-weight: bold;
    text-align: center;
    font-size: 25px;
    margin-top: 10px;
    background-color: #d4edda;
    padding: 10px;
    border-radius: 5px;
    border: 2px solid #c3e6cb;
}

body {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

p.error {
    background-color: #f8d7da;
    color: #721c24;
    padding: 10px;
    border-radius: 5px;
    font-weight: bold;
    margin: 3px 0;
}

p.error a {
    color: #721c24;
    font-weight: bold;
}

.input-group {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.input-group label {
    font-weight: bold;
}

.container {
    margin-top: 10px;
    display: flex;
    flex-direction: column;
    max-width: 800px;
    margin: 0 auto;
}

.container h1 {
    font-size: 25px;
    color: #1e78a6;
    font-weight: 700;
    margin-bottom: 0;
}

input {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 16px;
}

</style>