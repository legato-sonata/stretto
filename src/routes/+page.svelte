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
		
		<!-- Explanation Section -->
		<div class="text-left w-full max-w-2xl bg-[#c0c0c0] border-4 border-t-white border-l-white border-b-gray-600 border-r-gray-600 p-6 mb-8 font-serif leading-relaxed">
			<h2 class="text-xl font-bold mb-4"><u>Logical Analysis</u></h2>
			<p class="mb-4">
				The premise of the query relies on a mathematical inconsistency. In standard base-10 arithmetic, the equations provided are false (e.g., 2 + 10 &ne; 13). The "+" symbol and "=" sign in this context do not represent standard mathematical addition and equality. Instead, they indicate a custom logical function designed for pattern recognition.
			</p>

			<h2 class="text-xl font-bold mb-4 mt-6"><u>Pattern Identification</u></h2>
			<p class="mb-4">
				The underlying rule governing this dataset is that the final result is exactly 1 greater than the standard sum of the two numbers provided.
				If we define the left side of the equation as variables <i>x</i> and <i>y</i>, the pattern functions according to the following formula:
			</p>

			<div class="my-6 p-4 bg-white border-4 border-t-gray-600 border-l-gray-600 border-b-white border-r-white text-center text-2xl font-serif overflow-x-auto">
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

			<p class="mb-2">Applying this formula to the provided data validates the pattern:</p>
			<ul class="list-square list-inside ml-4 space-y-2 mb-4">
				<li>For 2 + 10: <b>(2 + 10) + 1 = 13</b></li>
				<li>For 2 + 8: <b>(2 + 8) + 1 = 11</b></li>
				<li>For 6 + 10: <b>(6 + 10) + 1 = 17</b></li>
			</ul>
			<p class="mt-4">
				<i>The pattern is a standard addition operation incremented by a constant value of one.</i>
			</p>
		</div>
	</center>
</div>
