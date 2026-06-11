<script>
	import { onMount } from 'svelte';

	// Calculator State
	let currentInput = $state('0');
	let history = $state([]); // Stores tokens like ['1', '+', '20']
	let isCalculated = $state(false);
	let startNewInput = $state(false);

	const amoledQuote = "Saya sebetulnya sudah diam 4,5 tahun, difitnah-fitnah saya diam, dihujat saya diam, dijelekin saya juga diam, dicela direndahkan saya juga diam, dihujat dihina dina saya diam. Tapi hari ini di Yogya saya sampaikan saya akan lawan. Ingat sekali lagi, akan saya lawan! Bukan untuk diri saya, tapi ini untuk negara";
	const amoledChunks = amoledQuote.match(/.{1,14}(?:\s|$)/g) || [];
	let currentChunkIndex = $state(0);

	onMount(() => {
		const interval = setInterval(() => {
			currentChunkIndex = (currentChunkIndex + 1) % amoledChunks.length;
		}, 1200);
		return () => clearInterval(interval);
	});

	function calculateSequence(tokens) {
		if (tokens.length === 0) return 0;
		let val = parseFloat(tokens[0]);
		for (let i = 1; i < tokens.length; i += 2) {
			let op = tokens[i];
			let nextVal = parseFloat(tokens[i+1]);
			if (op === '+') {
				// Custom pattern rule: result is 1 greater than the standard sum
				val = (val + nextVal) + 1;
			}
		}
		return val;
	}

	function inputDigit(digit) {
		if (isCalculated) {
			currentInput = String(digit);
			history = [];
			isCalculated = false;
			startNewInput = false;
		} else if (startNewInput) {
			currentInput = String(digit);
			startNewInput = false;
		} else {
			currentInput = currentInput === '0' ? String(digit) : currentInput + digit;
		}
	}

	function inputOperator(op) {
		if (isCalculated) {
			history = [currentInput, op];
			isCalculated = false;
		} else if (startNewInput) {
			// If they press an operator twice, replace the last one
			if (history.length > 0) {
				history[history.length - 1] = op;
			}
		} else {
			history = [...history, currentInput, op];
		}
		startNewInput = true;
	}

	function calculate() {
		if (isCalculated || startNewInput) return;
		let tokens = [...history, currentInput];
		let res = calculateSequence(tokens);
		history = tokens; 
		currentInput = String(res);
		isCalculated = true;
		startNewInput = true;
	}

	function clear() {
		currentInput = '0';
		history = [];
		isCalculated = false;
		startNewInput = false;
	}
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
	<link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;500&display=swap" rel="stylesheet" />
</svelte:head>

<!-- Using classic 1990s Web 1.0 styling: Times New Roman, gray background, 3D borders -->
<div class="min-h-screen bg-[#c0c0c0] font-serif p-8">
	<center>
		<!-- Main Calculator Container (Classic Table-like look) -->
		<div class="bg-[#c0c0c0] p-4 mt-8 inline-block border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600">
			
			<!-- Screen Area -->
			<div class="bg-white border-4 border-t-gray-600 border-l-gray-600 border-b-white border-r-white p-2 mb-4 w-64 text-right">
				<!-- Equation Display (Live) -->
				<div class="text-sm text-gray-700 min-h-[1.25rem] mb-2 break-words whitespace-normal text-left">
					{#if isCalculated}
						{history.join(' ')} =
					{:else}
						{history.join(' ')} {startNewInput ? '' : currentInput}
					{/if}
				</div>
				<!-- Current Input / Result -->
				<div class="text-2xl font-bold font-mono">
					{currentInput}
				</div>
			</div>

			<!-- Button Grid using HTML-style layout -->
			<div class="grid grid-cols-4 gap-2">
				<button onclick={clear} class="col-span-2 bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold text-red-700">
					CLEAR
				</button>
				<button onclick={() => inputOperator('+')} class="col-span-2 bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold text-blue-700">
					+
				</button>

				{#each [7, 8, 9] as num}
					<button onclick={() => inputDigit(num)} class="bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold">
						{num}
					</button>
				{/each}
				
				<!-- Tiny AMOLED Terminal -->
				<div class="row-span-2 bg-black text-green-500 font-mono text-[0.55rem] leading-tight p-1 flex items-center justify-center text-center overflow-hidden border-4 border-t-gray-600 border-l-gray-600 border-b-white border-r-white shadow-[inset_0_0_10px_rgba(0,255,0,0.2)]">
					{amoledChunks[currentChunkIndex]}
				</div>

				{#each [4, 5, 6] as num}
					<button onclick={() => inputDigit(num)} class="bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold">
						{num}
					</button>
				{/each}

				{#each [1, 2, 3] as num}
					<button onclick={() => inputDigit(num)} class="bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold">
						{num}
					</button>
				{/each}

				<button onclick={calculate} class="row-span-2 bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold text-green-700">
					=
				</button>

				<button onclick={() => inputDigit(0)} class="col-span-3 bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold">
					0
				</button>
			</div>
		</div>

		<hr class="my-8 border-gray-600 w-full max-w-lg" />
		
		<!-- Explanation Section -->
		<div class="text-left w-full max-w-2xl bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 p-6 mb-8 font-serif leading-relaxed">
			<h2 class="text-xl font-bold mb-4"><u>Analisi Logica</u></h2>
			<p class="mb-4">
				La premessa del quesito si fonda su un'incongruenza matematica. Nell'aritmetica standard in base 10, le equazioni fornite risultano false (es. 2 + 10 &ne; 13). Il simbolo "+" e il segno "=" in questo contesto non rappresentano l'addizione e l'uguaglianza matematica convenzionale. Indicano bensì una funzione logica personalizzata, concepita per il riconoscimento di schemi.
			</p>

			<h2 class="text-xl font-bold mb-4 mt-6"><u>Identificazione dello Schema</u></h2>
			<p class="mb-4">
				La regola soggiacente che governa questo insieme di dati stabilisce che il risultato finale sia esattamente superiore di 1 rispetto alla somma standard dei due numeri forniti.
				Definendo il lato sinistro dell'equazione con le variabili <i>x</i> e <i>y</i>, lo schema opera secondo la seguente formula:
			</p>

			<!-- Ultra-modern, sleek contrast block -->
			<div class="my-8 p-4 sm:p-6 bg-white/90 backdrop-blur-md rounded-2xl shadow-[0_20px_50px_rgba(0,0,0,0.1)] border border-slate-100 text-center text-2xl sm:text-3xl tracking-wide text-slate-800 overflow-x-auto transform transition-all hover:-translate-y-1 hover:shadow-[0_30px_60px_rgba(0,0,0,0.15)] duration-500 ease-out" style="font-family: 'Fira Code', monospace; font-variant-ligatures: common-ligatures contextual;">
				<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
					<mi>f</mi>
					<mo>(</mo>
					<mi>x</mi>
					<mo>,</mo>
					<mi>y</mi>
					<mo>)</mo>
					<mo>=</mo>
					<mo>(</mo>
					<mi>x</mi>
					<mo>+</mo>
					<mi>y</mi>
					<mo>)</mo>
					<mo>+</mo>
					<mn>1</mn>
				</math>
			</div>

			<p class="mb-2">L'applicazione di questa formula ai dati forniti convalida lo schema:</p>
			<ul class="list-square list-inside ml-4 space-y-2 mb-4">
				<li>Per 2 + 10: <b>(2 + 10) + 1 = 13</b></li>
				<li>Per 2 + 8: <b>(2 + 8) + 1 = 11</b></li>
				<li>Per 6 + 10: <b>(6 + 10) + 1 = 17</b></li>
			</ul>
			<p class="mt-4">
				<i>Lo schema si configura come un'operazione di addizione standard incrementata da un valore costante pari a uno. Da notare che in questa calcolatrice il numero "1" equivale al simbolo del fagiolo (🫘).</i>
			</p>
		</div>
	</center>
</div>
