<script>
	// Calculator State
	let currentDisplay = $state('0');
	let previousValue = $state(null);
	let operator = $state(null);
	let waitingForNewValue = $state(false);

	function inputDigit(digit) {
		if (waitingForNewValue) {
			currentDisplay = String(digit);
			waitingForNewValue = false;
		} else {
			// Prevent multiple leading zeros or multiple decimals if we add them later
			currentDisplay = currentDisplay === '0' ? String(digit) : currentDisplay + digit;
		}
	}

	function inputOperator(op) {
		if (operator && !waitingForNewValue) {
			calculate();
		}
		previousValue = currentDisplay;
		operator = op;
		waitingForNewValue = true;
	}

	function calculate() {
		if (!operator || previousValue === null) return;
		
		const a = parseFloat(previousValue);
		const b = parseFloat(currentDisplay);
		let result = 0;
		
		if (operator === '+') {
			result = a + b;
		}
		
		currentDisplay = String(result);
		operator = null;
		previousValue = null;
		waitingForNewValue = true;
	}

	function clear() {
		currentDisplay = '0';
		previousValue = null;
		operator = null;
		waitingForNewValue = false;
	}
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
	<link href="https://fonts.googleapis.com/css2?family=VT323&display=swap" rel="stylesheet" />
</svelte:head>

<div class="min-h-screen bg-slate-800 flex items-center justify-center p-4 font-sans">
	
	<!-- Calculator Body -->
	<div class="bg-[#dcd6ca] p-6 rounded-2xl shadow-2xl border-4 border-[#b5ae9e] w-full max-w-sm">
		<!-- Brand Label -->
		<div class="text-[#5a554b] font-bold text-xl mb-4 tracking-widest">STRETTO</div>

		<!-- Screen -->
		<div 
			class="bg-[#8b9b8b] p-4 rounded-lg shadow-inner mb-6 border-4 border-[#7a887a] overflow-hidden"
			style="font-family: 'VT323', monospace;"
		>
			<div class="text-right text-[#2d332d] text-6xl tracking-widest truncate">
				{currentDisplay}
			</div>
		</div>

		<!-- Button Grid -->
		<div class="grid grid-cols-4 gap-3">
			
			<!-- Row 1 -->
			<button onclick={clear} class="col-span-2 bg-[#d14b4b] hover:bg-[#b03d3d] text-white font-bold py-4 rounded shadow-[0_4px_0_#8c2c2c] active:shadow-none active:translate-y-1 transition-all">
				C
			</button>
			<button onclick={() => inputOperator('+')} class="col-span-2 bg-[#5a554b] hover:bg-[#433f38] text-white font-bold text-2xl py-4 rounded shadow-[0_4px_0_#38352f] active:shadow-none active:translate-y-1 transition-all">
				+
			</button>

			<!-- Row 2 -->
			{#each [7, 8, 9] as num}
				<button onclick={() => inputDigit(num)} class="bg-[#b5ae9e] hover:bg-[#a19989] text-[#2c2a25] font-bold text-2xl py-4 rounded shadow-[0_4px_0_#8f897b] active:shadow-none active:translate-y-1 transition-all">
					{num}
				</button>
			{/each}
			
			<!-- Filler for missing ops to keep grid square for now -->
			<button disabled class="bg-[#9e9786] opacity-50 cursor-not-allowed text-[#2c2a25] font-bold text-xl py-4 rounded shadow-[0_4px_0_#8f897b]">
				
			</button>

			<!-- Row 3 -->
			{#each [4, 5, 6] as num}
				<button onclick={() => inputDigit(num)} class="bg-[#b5ae9e] hover:bg-[#a19989] text-[#2c2a25] font-bold text-2xl py-4 rounded shadow-[0_4px_0_#8f897b] active:shadow-none active:translate-y-1 transition-all">
					{num}
				</button>
			{/each}
			
			<button disabled class="bg-[#9e9786] opacity-50 cursor-not-allowed text-[#2c2a25] font-bold text-xl py-4 rounded shadow-[0_4px_0_#8f897b]">
				
			</button>

			<!-- Row 4 -->
			{#each [1, 2, 3] as num}
				<button onclick={() => inputDigit(num)} class="bg-[#b5ae9e] hover:bg-[#a19989] text-[#2c2a25] font-bold text-2xl py-4 rounded shadow-[0_4px_0_#8f897b] active:shadow-none active:translate-y-1 transition-all">
					{num}
				</button>
			{/each}
			
			<!-- Equals Button taking 2 rows -->
			<button onclick={calculate} class="row-span-2 bg-[#e0983d] hover:bg-[#c28130] text-white font-bold text-3xl py-4 rounded shadow-[0_4px_0_#9c6725] active:shadow-none active:translate-y-1 transition-all">
				=
			</button>

			<!-- Row 5 -->
			<button onclick={() => inputDigit(0)} class="col-span-3 bg-[#b5ae9e] hover:bg-[#a19989] text-[#2c2a25] font-bold text-2xl py-4 rounded shadow-[0_4px_0_#8f897b] active:shadow-none active:translate-y-1 transition-all">
				0
			</button>
		</div>
	</div>
</div>
