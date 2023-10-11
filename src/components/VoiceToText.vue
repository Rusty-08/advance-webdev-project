<script setup>
    import { ref, onMounted } from 'vue'
const transcript = ref('')
const isRecording = ref(false)

const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition
const sr = new Recognition()

onMounted(() => {
	sr.continuous = true
	sr.interimResults = true

	sr.onstart = () => {
		console.log('SR Started')
		isRecording.value = true
	}

	sr.onend = () => {
		console.log('SR Stopped')
		isRecording.value = false
	}

	sr.onresult = (evt) => {
		for (let i = 0; i < evt.results.length; i++) {
			const result = evt.results[i]

			if (result.isFinal) CheckForCommand(result)
		}

		const t = Array.from(evt.results)
			.map(result => result[0])
			.map(result => result.transcript)
			.join('')
		
		transcript.value = t
	}
})

const CheckForCommand = (result) => {
	const t = result[0].transcript;
	if (t.includes('stop recording')) {
		sr.stop()
	} else if (
		t.includes('what is the time') ||
		t.includes('what\'s the time')
	) {
		sr.stop()
		alert(new Date().toLocaleTimeString())
		setTimeout(() => sr.start(), 100)
	}
}

const ToggleMic = () => {
	if (isRecording.value) {
		sr.stop()
	} else {
		sr.start()
	}
}
</script>

<template>
    <section class="tab-pane fade" id="voice-to-text" role="tabpanel" aria-labelledby="voice-to-text-link" tabindex="0">
            <div class="container-fluid vh-100 d-flex justify-content-center align-items-center">
                <div class="appDiv">
					<h1>Voice to Speech</h1>
						<div class="content">
							<button :class="`mic`" @click="ToggleMic">Record</button>
							<textarea class="transcript" v-text="transcript" name="" id="" cols="30" rows="10"></textarea>
							
						</div>
	            </div>
            </div>
    </section> 
</template>

<style setup>
.appDiv{
	height: 100vh;
	width: 100%;
	position: relative;
	display: flex;
	justify-content: center;
	align-items: center;
	border-radius: 10px;
	border: solid 1px red;
	flex-direction: column;
}
.content{
	height: 80%;
	width: 90%;
	display: flex;
	align-items: center;
	justify-content: center;
	border: 1px solid red;
}
.appDiv textarea{
	height:80%;
	width: 60%;
	justify-self: flex-end;
}

</style>