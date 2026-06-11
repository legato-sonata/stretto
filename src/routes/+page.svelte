<script>
	// Calculator State
	let currentInput = $state('0');
	let history = $state([]); // Stores tokens like ['1', '+', '20']
	let isCalculated = $state(false);
	let startNewInput = $state(false);

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

<!-- Using classic 1990s Web 1.0 styling: Times New Roman, gray background, 3D borders -->
<div class="min-h-screen bg-[#c0c0c0] font-serif p-8">
	<center>
		<h1 class="text-4xl mb-4 font-bold"><u>Stretto Calculator</u></h1>
		<p class="mb-8">A barebones calculation utility for the World Wide Web.</p>

		<!-- Main Calculator Container (Classic Table-like look) -->
		<div class="bg-[#c0c0c0] p-4 inline-block border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600">
			
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
				
				<div class="bg-[#c0c0c0]"></div>

				{#each [4, 5, 6] as num}
					<button onclick={() => inputDigit(num)} class="bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 active:border-t-gray-600 active:border-l-gray-600 active:border-b-white active:border-r-white py-2 font-bold">
						{num}
					</button>
				{/each}
				
				<div class="bg-[#c0c0c0]"></div>

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
		<p class="text-sm"><i>Best viewed in NCSA Mosaic or Netscape Navigator.</i></p>
	</center>
</div>
