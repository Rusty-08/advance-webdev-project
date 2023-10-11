<script setup>
import { ref} from 'vue'

        const input = ref('')

        const Talk = (text) => {
            let inputSpeech = new SpeechSynthesisUtterance(text)
            speechSynthesis.speak(inputSpeech)
        }

     
</script>

<template>
    <section class="tab-pane fade" id="text-to-voice" role="tabpanel" aria-labelledby="text-to-voice-link" tabindex="0">
            <div class="container-fluid vh-100 d-flex justify-content-center align-items-center">
                    <div class="card" style="width: 18rem;">
                    <div class="card-body">
                    <h5 class="card-title">Card title</h5>
                    <textarea v-model="input" class="input w-100" id="input" cols="10" rows="5" ></textarea>
                    <button 
                        id="speak" 
                        class="btn btn-primary w-50"
                        @click="Talk(input)"
                    >
                        Speak
                </button>
            </div>
        </div>
            </div>
    </section> 
</template>

<style setup>

</style>