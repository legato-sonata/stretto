<script>
	import { onMount } from 'svelte';

	let currentInput = $state('0');
	let history = $state([]);
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

<div class="min-h-screen bg-[#c0c0c0] font-serif p-8">
	<center>
		<div class="bg-[#c0c0c0] p-4 mt-8 inline-block border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600">
			
			<div class="bg-white border-4 border-t-gray-600 border-l-gray-600 border-b-white border-r-white p-2 mb-4 w-64 text-right">
				<div class="text-sm text-gray-700 min-h-[1.25rem] mb-2 break-words whitespace-normal text-left">
					{#if isCalculated}
						{history.join(' ')} =
					{:else}
						{history.join(' ')} {startNewInput ? '' : currentInput}
					{/if}
				</div>
				<div class="text-2xl font-bold font-mono">
					{currentInput}
				</div>
			</div>

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
		
		<div class="text-left w-full max-w-2xl bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 p-6 mb-8 font-serif leading-relaxed">
			<h1 class="text-3xl font-bold mb-2">Custom Incremented Addition</h1>
			<p class="mb-6 font-bold text-[#008000]">Difficulty: Easy</p>

			<h3 class="text-xl font-bold mb-2 border-b border-gray-500 pb-1">Description</h3>
			<p class="mb-4">
				Given two integers <code>x</code> and <code>y</code>, return the result of a custom addition operation. 
			</p>
			<p class="mb-4">
				The custom operation requires calculating the standard sum of the two input integers and then incrementing that sum by exactly 1.
			</p>

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

			<h3 class="text-xl font-bold mb-2 border-b border-gray-500 pb-1">Examples</h3>
			
			<div class="mb-6">
				<p class="font-bold mb-1">Example 1:</p>
				<div class="bg-gray-100 border border-gray-400 p-3 font-mono text-sm mb-2">
					<strong>Input:</strong> x = 2, y = 10<br>
					<strong>Output:</strong> 13<br>
					<strong>Explanation:</strong> The standard sum of 2 and 10 is 12. Incrementing the sum by 1 yields 13.
				</div>
			</div>

			<div class="mb-6">
				<p class="font-bold mb-1">Example 2:</p>
				<div class="bg-gray-100 border border-gray-400 p-3 font-mono text-sm mb-2">
					<strong>Input:</strong> x = 2, y = 8<br>
					<strong>Output:</strong> 11<br>
					<strong>Explanation:</strong> The standard sum of 2 and 8 is 10. Incrementing the sum by 1 yields 11.
				</div>
			</div>

			<div class="mb-6">
				<p class="font-bold mb-1">Example 3:</p>
				<div class="bg-gray-100 border border-gray-400 p-3 font-mono text-sm mb-2">
					<strong>Input:</strong> x = 6, y = 10<br>
					<strong>Output:</strong> 17<br>
					<strong>Explanation:</strong> The standard sum of 6 and 10 is 16. Incrementing the sum by 1 yields 17.
				</div>
			</div>

			<h3 class="text-xl font-bold mb-2 border-b border-gray-500 pb-1">Constraints</h3>
			<ul class="list-square list-inside ml-4 mt-2">
				<li>
					<math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" class="text-lg">
						<mo>&#x2212;</mo><msup><mn>10</mn><mn>4</mn></msup>
						<mo>&#x2264;</mo><mi>x</mi><mo>,</mo><mi>y</mi>
						<mo>&#x2264;</mo><msup><mn>10</mn><mn>4</mn></msup>
					</math>
				</li>
			</ul>
			<div class="mt-8 text-center">
				<a href="https://github.com" class="text-blue-700 font-bold hover:underline">GitHub</a>
			</div>
		</div>
	</center>
</div>
