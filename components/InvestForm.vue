<template>
    <div class="form">
        <div class="form-content">
            <div class="form-group">
                <label class="form-label" for="currentValue">Valor atual investido:</label>
                <input v-model="currentValue" class="form-control" name="currentValue" type="text" v-on:keypress="onlyNumber" placeholder="Ex.: 3500">
            </div>
            <div class="form-group">
                <label class="form-label" for="monthlyInvestment">Valor investido mensalmente:</label>
                <input v-model="monthlyInvestment" class="form-control" name="monthlyInvestment" type="text" v-on:keypress="onlyNumber" placeholder="Ex.: 1250">
            </div>
            <div class="form-group">
                <label class="form-label" for="monthlyIncome">Rendimento mensal %:</label>
                <input v-model="monthlyIncome" class="form-control" name="monthlyIncome" type="text" v-on:keypress="onlyNumber" placeholder="Ex.: 0,85">
            </div>
            <div class="form-group">
                <label class="form-label" for="monthlyTime">Duração do investimento:</label>
                <input v-model="monthlyTime" class="form-control" name="monthlyTime" type="text" onkeypress="return event.charCode >= 48 && event.charCode <= 57" placeholder="Ex.: 12">
                <small class="form-text text-muted">Em meses</small>
            </div>

            <div class="form-group btn-content">
                <button class="btn btn-secondary" @click="calculate">Calcular</button>
                <a v-if="download" @click="downloadFile">Download</a>
            </div>
        </div>
    </div>
</template>

<script>
    import api from '../api/api'

    export default {
        name: 'InvestForm',
        data() {
            return {
                currentValue: '',
                monthlyInvestment: '',
                monthlyIncome: '',
                monthlyTime: '',
                download: false
            }
        },
        methods: {
            onlyNumber(evt) {
                let event = evt || window.event,
                    key = event.keyCode || event.which,
                    regex = /^[0-9.,]+$/

                key = String.fromCharCode(key);
                if( !regex.test(key) ) {
                    event.returnValue = false;
                    if(event.preventDefault) event.preventDefault();
                }
            },

            calculate() {
                console.log('ola')
                api.post('/',
                    {
                        currentValue: this.currentValue,
                        monthlyInvestment: this.monthlyInvestment,
                        monthlyIncome: this.monthlyIncome,
                        monthlyTime: this.monthlyTime
                    }
                )

                this.download = true
            },

            downloadFile() {
                this.download = false
                window.location.href = `${process.env.baseURL}/download`
            },
        }
    }
</script>

<style>
    .form {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
        background-color: rgb(38, 31, 65);
    }

    small {
        font-size: 12px;
    }

    .btn {
        padding: 6px 24px;
    }

    .btn-content {
        display: flex;
        gap: 12px;
    }

    .btn-content > a {
        color: #fff;
        align-self: flex-end;
    }
</style>