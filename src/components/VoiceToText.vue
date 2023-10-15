<script setup>
	import { faRotateLeft, faCheck, faCopy, faMicrophone, faStop, faArrowRotateLeft } from '@fortawesome/free-solid-svg-icons';
    import { ref, onMounted } from 'vue'

	let transcript = ref('')
	let isRecording = ref(false)
	let isTextCopied = ref(false)

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
		isTextCopied.value = !isTextCopied.value
		setTimeout(() => {
			isTextCopied.value = !isTextCopied.value
		}, 2000)
	}

	const clearBoard = () => {
		transcript.value = ''
		sr.stop()
		if(isRecording.value === true) {
			setTimeout(() => {
				sr.start()
			}, 1000)
		}
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
				<span v-if="!isRecording" class="fs-7 m-0">Start Recording</span>
				<span v-else class="fs-7 m-0">Recording...</span>
                <p class="fs-8 mb-4"><span class="fw-medium">Note:</span> You can only record in English Language</p>
				<div class="text-output position-relative card h-50 text-start w-75 shadow-sm p-4" rows="10" readonly>
					<span class="recorded-text">{{ transcript }}</span>
					<div class="command-button gap-2 d-flex align-items-center position-absolute">
						<button 
							class="btn reset-board px-4 d-flex align-items-center gap-1"
							@click="clearBoard()"
						>
							<p class="fs-8 m-0">Clear</p>
							<font-awesome-icon
								:icon="faArrowRotateLeft"
                    	        class="fs-6"
                    	    />
						</button>
						<button 
							class="copy-voice-text px-4 btn d-flex align-items-center justify-content-center gap-1"
							@click="copyRecordedText(transcript)"
						>
							<p class="fs-8 m-0 text-light">Copy</p>
							<font-awesome-icon 
								:icon="faCopy"
                    	        class="fs-6 text-center" 
                    	        v-if="isTextCopied == false"
                    	    />
                    	    <font-awesome-icon 
                    	        :icon="faCheck" 
                    	        class="fs-6 text-center" 
                    	        v-else
                    	    />
						</button>
					</div>
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
	.text-output {
		background-color: var(--background-color);
		border-color: var(--secondary-text-color);
		padding-bottom: 1rem !important;
	}
	.recorded-text {
		overflow-y: scroll;
		text-align: justify;
	}
	.recorded-text::-webkit-scrollbar {
        width: 6px;
    }
    .recorded-text::-webkit-scrollbar-thumb {
        background-color: transparent;
        border-radius: 6px;
    }
    .recorded-text:hover::-webkit-scrollbar-thumb {
        background-color: var(--secondary-text-color);
    }
	.command-button {
		top: -1.5rem;
		right: 1rem;
	}
	.command-button button p {
		font-weight: 500;
	}
	.command-button button {
		transition: var(--transition-175s);
		background-color: var(--secondary-text-color);
		height: 2.4rem;
	}
	.command-button button,
	.command-button button:hover svg {
		color: var(--primary-text-color);
	}
	.copy-voice-text:hover,
	.copy-voice-text:active {
		background-color: var(--tertiary-text-color) !important;
		border-color: transparent !important;
	}
	.command-button button svg {
		width: 1rem;
	}
	.reset-board {
		background-color: var(--primary-text-color) !important;
		color: var(--secondary-text-color) !important;
		border-color: var(--secondary-text-color) !important;
	}
	.reset-board:hover,
	.reset-board:active {
		border-color: var(--tertiary-text-color) !important;
	}
	.reset-board:hover svg,
	.reset-board:hover p {
		color: var(--tertiary-text-color) !important;
	}
</style>