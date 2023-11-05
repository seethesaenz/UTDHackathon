<script setup>
import { ref, computed } from 'vue'
import { Doughnut } from 'vue-chartjs'
import { Chart, ArcElement, Tooltip, Legend} from 'chart.js'


Chart.register(ArcElement, Tooltip, Legend)

//csv id | grossMonthlyIncome | monthlyCreditCardPayment | carPayment | studentLoanPayment | appraisedValue | downPayment | loanAmount | monthlyMortgage | creditScore
const accepted = ref(0)
const deniedByLTV = ref(0)
const deniedByDTI = ref(0)
const deniedByCredit = ref(0)
const fileUploaded = ref(false)
const fileInput = ref(null)


const options = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
        legend: {
            display: true,
            position: 'bottom',
            labels: {
                padding: 5,
                boxWidth: 20,
            }
        }
    }
}

const config = computed(() =>  ({
    labels: [
        'Accepted',
        'Denied by Credit',
        'Denied by LTV',
        'Denied by DTI'
    ],
    datasets: [{
        label: 'graph',
        data: [accepted.value, deniedByCredit.value, deniedByLTV.value, deniedByDTI.value],
        backgroundColor: [
        'rgb(0, 255, 0)',
        'rgb(54, 162, 235)',
        'rgb(255, 205,86)',
        'rgb(255, 99, 132)'
        ],
        hoverOffset: 4
        

    }]
}));

function onFileDrop(e) {
    e.preventDefault()
    onFileChange({ target: { files: e.dataTransfer.files }})
}

function onFileChange(e){
    const file = e.target.files[0]
    if (!file) return
    const reader = new FileReader()

    reader.onload = (e) => {
        //reset values
        accepted.value = 0
        deniedByLTV.value = 0
        deniedByDTI.value = 0
        deniedByCredit.value = 0

        const str = e.target.result
        processCSV(str)
        fileUploaded.value = true
    }
    reader.readAsText(file)
}

const processCSV = (str, delim = ',') => {
    // split string to rows
    const rows = str.split('\n')

    for (let i = 1; i < rows.length; i++) {
        if (rows[i].trim() === '') continue

        const [
            id,
            grossMonthlyIncome,
            monthlyCreditCardPayment,
            carPayment,
            studentLoanPayment,
            appraisedValue,
            downPayment,
            loanAmount,
            monthlyMortgage,
            creditScore
        ] = rows[i].split(delim).map(Number);

        // calculate ltv, dti, and fedti
        const ltv = loanAmount / appraisedValue
        const dti = (carPayment + studentLoanPayment + monthlyMortgage + monthlyCreditCardPayment) / grossMonthlyIncome
        const fedti = monthlyMortgage / grossMonthlyIncome
        const ltvAcceptable = ltv <= 0.95
        const dtiAcceptable = dti <= 0.36
        const fedtiAcceptable = fedti <= 0.28
        const creditScoreAcceptable = creditScore >= 640



        if(!ltvAcceptable){
            deniedByLTV.value++
        }
        else if(!dtiAcceptable){
            deniedByDTI.value++
        }
        else if(!fedtiAcceptable){
            deniedByDTI.value++
        }
        else if(!creditScoreAcceptable){
            deniedByCredit.value++
        }


        if(ltvAcceptable && dtiAcceptable && fedtiAcceptable && creditScoreAcceptable){
            accepted.value++
        }
    }
}


</script>

<template>
    <div class="container">
        <h1>CSV Upload</h1>
        <div class = "input-container" @drop="onFileDrop" @dragover.prevent>
            <input type="file" id="csv" accept=".csv" @change="onFileChange" ref="fileInput" />
            <label for="csv">Drag and drop a CSV file here or click to upload.</label>
        </div>
        <div class = "table-container" v-if="fileUploaded">
            <table id = "tbl">
                <tr>
                    <th>Accepted</th>
                    <th>Denied by Credit</th>
                    <th>Denied by LTV</th>
                    <th>Denied by DTI</th>
                </tr>
                <tr>
                    <td>{{ accepted }}</td>
                    <td>{{ deniedByCredit }}</td>
                    <td>{{ deniedByLTV }}</td>
                    <td>{{ deniedByDTI }}</td>
                </tr>
            </table>
        </div>
        <div id = "chrt" v-if="fileUploaded">
            <Doughnut :data="config" :options="options" />
        </div>
    </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

}
body {
    max-width: 1000px;
    margin: 0 auto;
    padding: 20px;
}
table, th, td {
  border: 1px solid;
    padding: 5px;
    text-align: center;
}

table {
    border-collapse: collapse;
    margin: auto;
}
.table-container {
    display: flex;
    justify-content: center;
    align-items: center;

}

.input-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    border: 2px dashed #aaa;
    padding-bottom: 20px;
    margin-bottom: 20px;
    cursor: pointer;
}

.input-container:hover {
    border-color: #777;
    background-color: #777;
}
#chrt {
    padding-top: 20px;
    padding-bottom: 20px;
    width: 600px;
}
input[type="file"] {
    opacity: 0;
    position: absolute;
    z-index: -1;
}
</style>