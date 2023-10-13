<script setup>
import { faCheck, faClipboard, faDroplet, faLocationCrosshairs, faFill } from '@fortawesome/free-solid-svg-icons';
import { ref, watch } from 'vue'

let hex = ref('#4b5369')
let rgb = ref('')
let rgba = ref('')
let isRgbCopied = ref(false)
let isRgbaCopied = ref(false)

const updateColorValues = () => {
    
    let removedHash = 
        hex.value.charAt(0) != '#'
        ? hex.value.trim()
        : hex.value.trim().slice(1)
        
    if(removedHash.length == 3 || removedHash.length == 4) removedHash = transformHex(removedHash)
    
    if ( 
        removedHash.length < 3 || 
        removedHash.length == 5 || 
        removedHash.length == 7 || 
        removedHash.length > 8
    ) {
        rgb.value = 'Invalid Input'
        rgba.value = 'Invalid Input'
        return
    }

    const r = parseInt(removedHash.substring(0, 2), 16)
    const g = parseInt(removedHash.substring(2, 4), 16)
    const b = parseInt(removedHash.substring(4, 6), 16)
    const a = parseInt(removedHash.substring(6, 8), 16) / 255

    if (isNaN(r) || isNaN(g) || isNaN(b)) {
        rgb.value = 'Invalid Input'
        rgba.value = 'Invalid Input'
        return
    } 
    
    if (removedHash.length === 6) {
        rgb.value = `rgb(${r}, ${g}, ${b})`
        rgba.value = `rgba(${r}, ${g}, ${b}, 1)`
    } else {
        rgb.value = `rgb(${r}, ${g}, ${b})`
        rgba.value = 
            a % 1 !== 0
            ? `rgba(${r}, ${g}, ${b}, ${a.toFixed(2)})`
            : `rgba(${r}, ${g}, ${b}, ${a})`
    }
}

// TRANSFORMING HEX WITH LENGTH OF 3 and 4 TO BE A VALID HEX

const transformHex = (hexValue) => {
    let newHex = hexValue.split('')
    let result = ''

    for(let i = 0; i < newHex.length; i++) {
        result += `${newHex[i]}${newHex[i]}`
    }
    return result
}

// RUN THE FUNCTION AS THE VALUE OF HEX CHANGES

watch(() => hex.value, updateColorValues)

// INITIALIZE THE DEFAULT HEX

updateColorValues()

const copyRgb = (textToCopy) => {
    navigator.clipboard.writeText(textToCopy)
    isRgbCopied.value = !isRgbCopied.value
    setTimeout(() => {
        isRgbCopied.value = !isRgbCopied.value
    }, 1000)
}

const copyRgba = (textToCopy) => {
    navigator.clipboard.writeText(textToCopy)
    isRgbaCopied.value = !isRgbaCopied.value
    setTimeout(() => {
        isRgbaCopied.value = !isRgbaCopied.value
    }, 1000)
};

</script>

<template>
    <section class="tab-pane fade show active" id="hex-to-rgba" role="tabpanel" aria-labelledby="hex-to-rgba-link" tabindex="0">
        <div class="container-fluid vh-100 p-4 py-3 shadow-sm">
            <div class="page-content gap-3 card p-4 w-100 h-100 d-flex justify-content-center align-items-center">
                <h2 class="fs-5">Hex to RGBA Converter</h2>
                <input
                    type="text"
                    class="hex-value form-control shadow-sm text-center fw-medium"
                    v-model="hex"
                >
                <p class="fs-8 m-0"><span class="fw-medium">Tip:</span> You can enter 8 character Hex code — #11223344 for transparency.</p>
                <div class="result card w-100 p-4 d-flex justify-content-center align-items-center">
                    <div class="rgb-result w-50 d-flex flex-column">
                        <h3 class="fs-6 text-center mb-3">CONVERTED</h3>
                        <div class="rgb-wrapper d-flex w-100 px-3 rounded-3 justify-content-between align-items-center">
                            <p
                                class="rgb mb-0 w-100 px-2 text-start fs-5 fw-medium"
                            >
                                <font-awesome-icon :icon="faFill" class="fs-5 px-1" />
                                {{ rgb }}
                            </p>
                            <button 
                                type="button" 
                                class="copy-result btn"
                                @click="copyRgb(rgb)"
                                v-show="rgb !== 'Invalid Input'"
                            >
                                <font-awesome-icon 
                                    :icon="faClipboard"
                                    class="fs-5 text-center p-0 m-0 opacity-50" 
                                    v-if="isRgbCopied == false"
                                />
                                <font-awesome-icon 
                                    :icon="faCheck" 
                                    class="check-icon fs-5 text-center p-0 m-0 opacity-50" 
                                    v-else
                                />
                            </button>
                        </div>
                        <div class="rgb-wrapper mt-3 d-flex w-100 px-3 mb-3 rounded-3 justify-content-between align-items-center">
                            <p 
                                class="rgba mb-0 w-100 px-2 text-start fs-5 fw-medium"
                            >  
                                <font-awesome-icon :icon="faFill" class="fs-5 px-1" />
                                {{ rgba }}
                            </p>
                            <button 
                                type="button" 
                                class="copy-result btn"
                                @click="copyRgba(rgba)"
                                v-show="rgba !== 'Invalid Input'"
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
                    </div>
                    <h3 class="fs-6 pt-3 border-top w-100 text-center">PREVIEW</h3>
                    <div :style="{ backgroundColor: rgba != 'Invalid Input' ? rgba : transparent }" class="color-preview card mt-2 shadow-sm"></div>
                </div>
            </div>
        </div>
    </section>
</template>


<style setup>
    .page-content {
        border-color: transparent;
    }
    .page-content h2 {
        color: var(--tertiary-text-color);
    }
    .page-content input {
        width: 10rem;
        border: 2px solid var(--secondary-text-color);
        color: var(--tertiary-text-color);
    }
    .page-content input:focus,
    .page-content textarea:focus {
        color: var(--dark-text-color) !important;
        border-color: var(--dark-text-color) !important;
    }
    .page-content p,
    .result h3 {
        color: var(--secondary-text-color);
    }
    .page-content span,
    .result p {
        color: var(--dark-text-color);;
    }
    .result p{
        cursor: pointer;
    }
    .result h3 {
        font-weight: 600;
    }
    .color-preview {
        width: 5rem;
        height: 5rem;
        border-color: transparent;
    }
    .rgb-wrapper {
        background-color: var(--background-color) !important;
        height: 4rem;
    }
    .rgb-wrapper svg {
        color: var(--secondary-text-color);
        margin-right: 0.2rem;
    }
    .copy-result {
        transition: var(--transition-175s);
    }
    .copy-result:hover,
    .copy-result:active {
        background-color: rgba(137, 146, 168, 0.1) !important;
        border-color: transparent !important;
    }
    .copy-result:active {
        background-color: rgba(137, 146, 168, 0.2) !important;
    }
    .copy-result .check-icon {
        color: var(--active-link-background-color);
    }

</style>