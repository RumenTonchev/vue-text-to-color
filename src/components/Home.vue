<template>
    <div>
        <div class="input-div">
            <input type="text" v-model="inputField">
        </div>
        <div class="output-div">
            <template v-for="letter of inputField" v-if="letterConvert(letter) !== 'not valid'">
                <div class="output-letter" :style="{background: letterConvert(letter)}">
                </div>
            </template>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Home",
        data() {
            return {
                inputField: '',
                outputArray: []
            }
        },
        methods: {
            letterConvert(letter) {
                let number = letter.toLowerCase().charCodeAt(0) - 97;
                if (number === -65) {
                    return 'transparent';
                } else if(number >=0 && number<= 25) {
                    let disperseNumber = Math.floor(number * 4095 / 26);
                    let resultString = disperseNumber.toString(16).padStart(3, '0');
                    return `#${resultString}`;
                } else {
                    return 'not valid';
                }
            },
        }
    }
</script>

<style scoped>
    .input-div, .output-div {
        width: 100%;
        padding: 30px;
    }

    .output-letter {
        display: inline-block;
        width: 20px;
        height: 20px;
        border: 1px solid transparent;
    }


</style>