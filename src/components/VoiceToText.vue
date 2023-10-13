<script setup>
	import { faCopy, faMicrophone, faStop } from '@fortawesome/free-solid-svg-icons';
    import { ref, onMounted } from 'vue'

	const transcript = ref('')
	const isRecording = ref(false)

	const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition
	const sr = new Recognition()

	onMounted(() => {
		sr.continuous = true
		sr.interimResults = true

		sr.onstart = () => {
			isRecording.value = true
		}

		sr.onend = () => {
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

	const record = () => {
		if(isRecording) {
			sr.stop()
		}
		sr.start()
	}

	const copyRecordedText = (textToCopy) => {
    	navigator.clipboard.writeText(textToCopy)
	};

</script>

<template>
    <section class="tab-pane fade" id="voice-to-text" role="tabpanel" aria-labelledby="voice-to-text-link" tabindex="0">
        <div class="container-fluid vh-100 p-4 py-3 shadow-sm">
            <div class="page-content gap-3 card p-4 w-100 h-100 d-flex justify-content-center align-items-center">
				<h2 class="fs-5">Voice to Text</h2>
				<button 
					@click="record()" 
					class="btn mic-btn"
					:class="{ 'stop-mode': isRecording }"
				>
					<font-awesome-icon v-if="!isRecording" :icon="faMicrophone" class="start-icon text-center fs-3 px-1" />
					<font-awesome-icon v-else :icon="faStop" class="stop-icon fs-3 text-center px-1" />
				</button>
				<p v-if="!isRecording" class="fs-7">Start Recording</p>
				<p v-else class="fs-7">Recording...</p>
				<div class="text-output position-relative card h-50 text-start w-75 shadow-sm p-4" rows="10" readonly>
					{{ transcript }}
					<button 
						class="copy-voice-text px-3 position-absolute btn d-flex align-items-center gap-1"
						v-show="transcript !== ''"
						@click="copyRecordedText(transcript)"
					>
						<p class="fs-8 m-0 text-light">Copy</p>
						<font-awesome-icon :icon="faCopy" class="fs-6" />
					</button>
				</div>
			</div>
        </div>
    </section> 
</template>

<style setup>
	.mic-btn {
		width: 4rem;
		height: 4rem;
		border-radius: 50%;
		background-color: var(--background-color);
		transition: var(--transition-175s);
	}
	.mic-btn:hover,
	.mic-btn:active {
		background-color: rgba(75, 83, 105, 0.2);
		border-color: transparent !important;
	}
	.mic-btn svg {
		color: var(--tertiary-text-color);
	}
	.stop-mode {
		background-color: rgba(255, 128, 128, 0.2);
	}
	.stop-mode:hover,
	.stop-mode:active {
		background-color: rgba(255, 128, 128, 0.3);
	}
	.stop-mode svg {
		color: #FF8080;
	}
	.text-output,
	.text-output:focus {
		border-color: transparent;
		background-color: var(--background-color);
	}
	.copy-voice-text {
		transition: var(--transition-175s);
		top: -1rem;
		right: 1.5rem;
		background-color: var(--secondary-text-color);
	}
	.copy-voice-text,
	.copy-voice-text:hover svg {
		color: var(--primary-text-color) !important;
	}
	.copy-voice-text:hover,
	.copy-voice-text:active {
		background-color: var(--tertiary-text-color);
		border-color: transparent !important;
	}
</style>