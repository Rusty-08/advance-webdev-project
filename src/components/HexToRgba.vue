<script setup>
import { ref, watch } from 'vue'

let hex = ref('#4b5369')
let rgb = ref('')
let rgba = ref('')

const updateColorValues = () => {
    
    let removedHash = 
        hex.value.charAt(0) != '#'
        ? hex.value
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
            a != 1
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

updateColorValues();

</script>

<template>
    <section class="tab-pane fade show active" id="hex-to-rgba" role="tabpanel" aria-labelledby="hex-to-rgba-link" tabindex="0">
        <div class="container-fluid vh-100 p-4 shadow-sm">
            <div class="hex-content gap-4 card p-5 w-100 h-100 d-flex justify-content-center align-items-center">
                <h2 class="fs-5">Hex to RGBA Converter</h2>
                <input
                    type="text"
                    class="form-control shadow-sm text-center fw-medium"
                    v-model="hex"
                >
                <span class="fs-8">Tip: You can enter 8 character Hex code — #11223344 for transparent RGBA colors.</span>
                <div class="result card w-100 p-4 d-flex justify-content-center align-items-center">
                    <h3 class="fs-6">CONVERTED</h3>
                    <p 
                        class="rgb mt-2 w-100 text-center fs-5 fw-medium"
                    >
                        {{ rgb }}
                    </p>
                    <p 
                        class="rgba w-100 text-center fs-5 pb-3 border-bottom fw-medium"
                    >
                        {{ rgba }}
                    </p>
                    <h3 class="fs-6">PREVIEW</h3>
                    <div :style="{ backgroundColor: rgba }" class="color-preview card mt-2 shadow-sm"></div>
                </div>
            </div>
        </div>
    </section>
</template>


<style setup>
    .hex-content {
        border-color: transparent;
    }
    .hex-content h2,
    .result h3 {
        color: var(--tertiary-text-color);
    }
    .hex-content input {
        width: 10rem;
        border: 2px solid var(--secondary-text-color);
        color: var(--tertiary-text-color);
    }
    .hex-content input:focus {
        color: var(--dark-text-color);
        border-color: var(--dark-text-color);
    }
    .hex-content span {
        color: var(--secondary-text-color);
    }
    .result p {
        color: var(--dark-text-color);
        cursor: pointer;
    }
    .color-preview {
        width: 5rem;
        height: 5rem;
        border-color: transparent;
    }
</style>