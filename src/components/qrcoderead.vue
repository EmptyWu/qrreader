<script setup>
import { ref } from 'vue'
import { QrcodeStream } from 'vue-qrcode-reader';
const paused = ref(false);
const showScanConfirmation = ref(false);
const result = ref('');
const errrormsg=ref('');

const onCameraOff = () => {
    showScanConfirmation.value = true;
}
const onCameraOn = () => {
    showScanConfirmation.value = false;
}
const onDetect = async (detectedCodes) => {
    result.value = JSON.stringify(detectedCodes.map((code) => code.rawValue))

    paused.value = true
    await timeout(500)
    paused.value = false
}

const timeout = (ms) => {
    return new Promise((resolve) => {
        window.setTimeout(resolve, ms)
    })
};
const onError=()=>{
    errrormsg.value=console.error.toString();
}

</script>

<template>
    <div>
        <p class="decode-result">
            Last result: <b>{{ result }}</b><br/>
            error : <b>{{ errrormsg }}</b>
        </p>

        <qrcode-stream :paused="paused" @detect="onDetect" @camera-on="onCameraOn" @camera-off="onCameraOff"
            @error="onError">
            <div v-show="showScanConfirmation" class="scan-confirmation">
                <img src='/checkmark.svg' alt="Checkmark" width="128" />
            </div>
        </qrcode-stream>
    </div>
</template>

<style scoped>
.scan-confirmation {
    position: absolute;
    width: 100%;
    height: 100%;

    background-color: rgba(255, 255, 255, 0.8);

    display: flex;
    flex-flow: row nowrap;
    justify-content: center;
}
</style>