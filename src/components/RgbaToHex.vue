<script setup>
    import { faCheck, faClipboard, faDroplet, faFill } from '@fortawesome/free-solid-svg-icons';

    import { ref, watch, computed, watchEffect } from 'vue'
    
    let R = ref(75)
    let G = ref(83)
    let B = ref(105)
    let A = ref(1)

    let isRgbaCopied = ref(false)

    let hexValue = computed(() => rgbaToHex())
    
    let rgbaValue = computed({
        get: () => `rgba(${R.value}, ${G.value}, ${B.value}, ${A.value <= 1 && A.value >= 0 ? A.value : 1})`,
        set: (newValue) => {
            const rgbaParts = newValue.match(/\d+(\.\d+)?/g).map(value => parseFloat(value));
            R.value = rgbaParts[0]
            G.value = rgbaParts[1]
            B.value = rgbaParts[2]
            A.value = rgbaParts[3]
        }
    })
    
    const RGBA = [
        {label: 'R', color: R, placeholder: 'Red'},
        {label: 'G', color: G, placeholder: 'Green'},
        {label: 'B', color: B, placeholder: 'Blue'},
        {label: 'A', color: A, placeholder: 'Opacity'}
    ]

    const rgbaToHex = () => {
        const hexR = R.value.toString(16).padStart(2, '0')
        const hexG = G.value.toString(16).padStart(2, '0')
        const hexB = B.value.toString(16).padStart(2, '0')
        const hexA = Math.round(A.value * 255).toString(16).padStart(2, '0')

        const hexColor = `#${hexR}${hexG}${hexB}${A.value == 1 ? '' : hexA}`

        return hexColor
    }

    const copyRgba = (textToCopy) => {
        navigator.clipboard.writeText(textToCopy)
        isRgbaCopied.value = !isRgbaCopied.value
        setTimeout(() => {
            isRgbaCopied.value = !isRgbaCopied.value
        }, 1000)
    }

    watchEffect(() => {
        hexValue.value = rgbaToHex()
    });

</script>

<template>
    <section class="tab-pane fade" id="rgba-to-hex" role="tabpanel" aria-labelledby="rgba-to-hex-link" tabindex="0">
        <div class="container-fluid p-3 px-4 vh-100 d-flex justify-content-center align-items-center">
            <div class="page-content shadow-sm gap-3 card p-4 w-100 h-100 d-flex justify-content-center align-items-center">
                <h2 class="fs-5 mb-3">RGBA to Hex Converter</h2>
                <div class="d-flex gap-4 align-items-center justify-content-center">
                    <div 
                        class="rgba-colors gap-2 d-flex align-items-center justify-content-center"
                        v-for="(color, index) in RGBA"
                        :key="index"
                    >
                        <span class="fs-5">{{ color.label }}</span>
                        <input 
                            class="form-control shadow-sm fw-semibold"
                            type="number" 
                            v-model="color.color.value"
                            :placeholder="color.placeholder"
                            :step="color.label == 'A' ? 0.01: 1"
                            :max="color.label == 'A' ? 1 : 255"
                            min="0"
                        >
                    </div>
                </div>
                <div class="rgba-result d-flex align-items-center my-3">
                    <input 
                        type="text"
                        class="form-control shadow-sm text-center fw-semibold"
                        v-model="rgbaValue"
                    >
                </div>
                <div class="result card w-100 p-4 d-flex justify-content-center align-items-center">
                    <div class="rgb-result w-100 d-flex align-items-center flex-column">
                        <h3 class="fs-6 text-center mb-3">CONVERTED</h3>
                        <div class="hex-result gap-5 p-3 mb-4 rounded-3 d-flex align-items-center">
                            <p 
                                class="hex mb-0 px-2 w-100 text-start fs-5 fw-medium"
                            >
                                <font-awesome-icon :icon="faFill" class="fs-5" />
                                {{ hexValue }}
                            </p>
                            <button 
                                type="button" 
                                class="copy-result btn"
                                @click="copyRgba(hexValue)"
                            >
                                <font-awesome-icon 
                                    :icon="faClipboard"
                                    class="fs-5 text-center p-0 m-0 opacity-50" 
                                    v-if="isRgbaCopied == false"
                                />
                                <font-awesome-icon 
                                    :icon="faCheck" 
                                    class="check-icon fs-5 text-center p-0 m-0" 
                                    v-else
                                />
                            </button>
                        </div>
                        <h3 class="fs-6 pt-3 border-top w-100 text-center">PREVIEW</h3>
                        <div 
                            :style="{ backgroundColor: rgbaValue }"
                            class="color-preview card mt-2 shadow-sm">
                        </div>
                    </div>
                </div>
            </div> 
        </div>
    </section> 
</template>

<style setup>
    .rgba-colors span {
        font-weight: 500;
        color: var(--tertiary-text-color);
    }
    .rgba-colors input {
        width: 7rem !important;
    }
    .rgba-colors input::placeholder {
        opacity: 0.5;
    }
    .rgba-result input {
        width: 15rem;
        background-color: transparent;
        border-width: 1px;
    }
    .hex-result {
        background-color: var(--background-color);
    }
    .hex svg,
    .copy-result svg {
        color: var(--secondary-text-color);
    }
    .copy-result svg {
        width: 1rem;
    }

</style>