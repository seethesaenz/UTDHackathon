<script setup>
import { ref } from 'vue'

// Messages are in format:
// {
//     text: 'message text',
//     response: 'response text',
// }

const messages = ref([{ response: 'Hi, I\'m ' + 'FrannieBot' + '! How can I help you?' }])

// 1000 possible responses
const RESPONSES = [
  { match: /hi/i, response: 'Hi! What do you need help with?' },
  { match: /bye/i, response: 'It was nice talking to you! I hope this helped.' },
  { match: /(.*)loan/i, response: 'Do you qualify for a loan?' },
  { match: /yes/i, response: 'Good! Buy a house then.' },
  { match: /no/i, response: "What part of your loan qualification do you need help with? (e.g. Credit score, DTI, LTV)" },
  { match: /(.*)help/i, response: 'What do you need help with?' },
  { match: /(.*)improve(.*)Credit(.*)/i, response: 'If you want to improve your credit score, see this article: https://yourhome.fanniemae.com/own/managing-credit#:~:text=Managing%20your%20debt&text=Making%20a%20large%20purchase%20on,a%20new%20loan%20or%20mortgage' },
  { match: /(.*)lower(.*)DTI(.*)/i, response: "To lower DTI, I recommend this article: https://yourhome.fanniemae.com/buy/why-understanding-debt-essential" },
  { match: /(.*)lower(.*)LTV(.*)/i, response: "To lower LTV, I recommend this article: 'https://singlefamily.fanniemae.com/originating-underwriting/mortgage-products/97-loan-value-options'" },
  { match: /(.*)lower(.*)debt(.*)/i, response: "Here are some tips on lowering your debt: Pay off some debt or transfer debt to a lower interest rate loan/credit card" },
  { match: /What is DTI?/i, response: "DTI stands for 'Debt-to-Income' ratio. It is a financial metric used to assess a person's or household's financial health and ability to manage debt. The DTI ratio is typically expressed as a percentage and is calculated by dividing the total monthly debt payments by the total monthly gross income. It is often used by lenders, such as banks and mortgage companies, to evaluate a borrower's creditworthiness and determine their eligibility for loans." },
  { match: /What is(.*)LTV/i, response: 'LTV stands for "Loan-to-Value" ratio. It is a financial metric used in real estate and lending. LTV is calculated by dividing the loan amount by the appraised value of the property or the purchase price, whichever is lower.' },
];

function getResponse(message) {
    for (const { match, response } of RESPONSES) {
        if (match.test(message)) {
            return response
        }
    }
    return 'I do not understand. Please rephrase your question.'
}

function processMessage(message) {
    messages.value.push({
        text: message,
        response: '...'
    })

    setTimeout(() => {
        messages.value[messages.value.length - 1].response = getResponse(message)
    }, 1000)
}

const currentMessage = ref('')
const sendMessage = () => {
    processMessage(currentMessage.value)
    currentMessage.value = ''
}
const handleEnter = (e) => {
    if (e.key === 'Enter') {
        sendMessage()
    }
}
</script>

<template>
    <div class="chat">
        <div class="messages">
            <div v-for="message in messages" class="message">
                <div class="message-text">{{ message.text }}</div>
                <div class="message-response">{{ message.response }}</div>
            </div>
        </div>
        <div class="input">
            <input v-model="currentMessage" @keyup="handleEnter" type="text" placeholder="Type a message..." />
            <button @click="sendMessage">Send</button>
        </div>
    </div>
</template>

<style scoped>
.chat {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin: 50px 0;
}

.messages {
    display: flex;
    flex-direction: column;
    gap: 10px;
    overflow-y: scroll;
}

.message {
    display: flex;
    flex-direction: column;
    gap: 5px;
}

.message-text {
    font-weight: bold;
}

.message-response {
    color: #999;
}

.input {
    display: flex;
    gap: 10px;
}

.input input {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 16px;
}

.input button {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 16px;
    background-color: #1e78a6;
    color: white;
    font-weight: bold;
}

</style>