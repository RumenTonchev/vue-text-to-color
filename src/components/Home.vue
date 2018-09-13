<template>
    <div>
        <div class="text-to-speech" style="height: 0; overflow: hidden; padding: 0">
            <vue-speech @onTranscriptionEnd="onEnd" lang="bg"/>
        </div>
        <div class="output-div">
            {{inputField2}}
        </div>
        <div class="output-div">
            <!--<template v-for="letter of inputField2" v-if="letterConvert(letter) !== 'not valid'">-->
            <template v-for="letter of inputField2">
                <div class="output-letter" :style="{background: letterConvert(letter)}"></div>
            </template>
        </div>
        <hr>
        <div class="input-div">
            <input type="text" v-model="inputField">
        </div>
        <div class="output-div">
            <template v-for="letter of inputField" v-if="letterConvert(letter) !== 'not valid'">
                <div class="output-letter" :style="{background: letterConvert(letter)}"></div>
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
                inputField2: '',
                outputArray: [],
                speechText: ''
            }
        },
        methods: {
            letterConvert(letter) {
                let number = letter.toLowerCase().charCodeAt(0) - 97;
                if (number === -65) {
                    return 'transparent';
                } else if (number >= 0 && number <= 25) {
                    let disperseNumber = Math.floor((number * 4095) / 26);
                    let resultString = disperseNumber.toString(16).padStart(3, '0');
                    return `#${resultString}`;
                } else if (number >= 975 && number <= 1006){
                    number = number - 975;
                    let disperseNumber = Math.floor((number * 4095) / 30);
                    let resultString = disperseNumber.toString(16).padStart(3, '0');
                    console.log(resultString);
                    return `#${resultString}`;
                } else {
                    return 'not valid';
                }
            },
            onEnd({lastSentence, transcription}) {
                this.inputField2 = lastSentence;
                // `lastSentence` is the last sentence before the pause
                // `transcription` is the full array of sentences
            }
        }
    }
</script>

<style>
    .text-to-speech, .input-div, .output-div {
        width: 100%;
        padding: 10px;
    }

    .output-letter {
        display: inline-block;
        width: 20px;
        height: 20px;
        border: 1px solid transparent;
    }

    .text-to-speech p {
        font-size: 10px;
        margin: 1px;
    }


</style>