<script lang="ts">
	let columns = 8;
	let rows = 8;

	let gridColor = ['#7d99ad', '#dae3e7'];
	let showControlledCells = true;
	let showConflictAlert = true;
	let queenCount = 0;
	let conflictCount = 0;
	let isCalculating = false;

	let queenOnGrid = Array.from({ length: rows }, () => Array(columns).fill(false));
	let controlledCells = Array.from({ length: rows }, () => Array(columns).fill(false));

	function calculateControlledCells() {
		controlledCells = Array.from({ length: rows }, () => Array(columns).fill(false));

		for (let queenRow = 0; queenRow < rows; queenRow++) {
			for (let queenCol = 0; queenCol < columns; queenCol++) {
				if (queenOnGrid[queenRow][queenCol]) {
					for (let col = 0; col < columns; col++) {
						controlledCells[queenRow][col] = true;
					}

					for (let row = 0; row < rows; row++) {
						controlledCells[row][queenCol] = true;
					}

					for (let i = 0; i < Math.max(rows, columns); i++) {
						const row = queenRow - i;
						const col = queenCol - i;
						if (row >= 0 && row < rows && col >= 0 && col < columns) {
							controlledCells[row][col] = true;
						}

						const row2 = queenRow + i;
						const col2 = queenCol + i;
						if (row2 >= 0 && row2 < rows && col2 >= 0 && col2 < columns) {
							controlledCells[row2][col2] = true;
						}
					}

					for (let i = 0; i < Math.max(rows, columns); i++) {
						const row = queenRow - i;
						const col = queenCol + i;
						if (row >= 0 && row < rows && col >= 0 && col < columns) {
							controlledCells[row][col] = true;
						}

						const row2 = queenRow + i;
						const col2 = queenCol - i;
						if (row2 >= 0 && row2 < rows && col2 >= 0 && col2 < columns) {
							controlledCells[row2][col2] = true;
						}
					}
				}
			}
		}
	}

	function updateStats() {
		queenCount = queenOnGrid.flat().filter(Boolean).length;
		conflictCount = 0;

		for (let row = 0; row < rows; row++) {
			for (let col = 0; col < columns; col++) {
				if (queenOnGrid[row][col] && checkQueenConflicts(row, col)) {
					conflictCount++;
				}
			}
		}
	}

	function checkQueenConflicts(newQueenRow: number, newQueenCol: number): boolean {
		for (let row = 0; row < rows; row++) {
			for (let col = 0; col < columns; col++) {
				if (queenOnGrid[row][col] && (row !== newQueenRow || col !== newQueenCol)) {
					if (row === newQueenRow) return true;
					if (col === newQueenCol) return true;

					if (Math.abs(row - newQueenRow) === Math.abs(col - newQueenCol)) return true;
				}
			}
		}
		return false;
	}

	function toggleQueen(rowIndex: number, colIndex: number) {
		const wasQueenPresent = queenOnGrid[rowIndex][colIndex];

		if (!wasQueenPresent) {
			queenOnGrid[rowIndex][colIndex] = true;

			if (showConflictAlert && checkQueenConflicts(rowIndex, colIndex)) {
				alert('⚠️ Warning! This queen captures another queen on the board!');
			}
		} else {
			queenOnGrid[rowIndex][colIndex] = false;
		}

		calculateControlledCells();
		updateStats();
	}

	function resetBoard() {
		queenOnGrid = Array.from({ length: rows }, () => Array(columns).fill(false));
		controlledCells = Array.from({ length: rows }, () => Array(columns).fill(false));
		updateStats();
	}

	function solveNQueens() {
		resetBoard();
		isCalculating = true;

		// Utiliser setTimeout pour permettre à l'UI de se mettre à jour
		setTimeout(() => {
			const solution = findSolution(0, []);
			if (solution) {
				for (let i = 0; i < solution.length; i++) {
					queenOnGrid[i][solution[i]] = true;
				}
				calculateControlledCells();
				updateStats();
			} else {
				alert('No solution found for this board size.');
			}
			isCalculating = false;
		}, 50);
	}

	function findSolution(row: number, positions: number[]): number[] | null {
		if (row === rows) {
			return positions;
		}

		for (let col = 0; col < columns; col++) {
			if (isValidPosition(row, col, positions)) {
				const result = findSolution(row + 1, [...positions, col]);
				if (result) return result;
			}
		}
		return null;
	}

	function isValidPosition(row: number, col: number, positions: number[]): boolean {
		for (let i = 0; i < positions.length; i++) {
			const prevCol = positions[i];
			if (prevCol === col || Math.abs(i - row) === Math.abs(prevCol - col)) {
				return false;
			}
		}
		return true;
	}

	function boardSizeChanged(newSize: number) {
		console.log('Board size changed to:', newSize);
		columns = newSize;
		rows = newSize;
		resetBoard();
	}

	updateStats();
</script>

<main>
	<h1>8 Queens Problem</h1>
	<div class="global-container">
		<div class="grid-container">
			<div
				class="container"
				style="grid-template-columns: repeat({columns}, 1fr); grid-template-rows: repeat({rows}, 1fr);"
			>
				{#each Array(rows) as _, rowIndex}
					<div class="row">
						{#each Array(columns) as _, colIndex}
							<button
								class="cell"
								class:controlled={showControlledCells &&
									controlledCells[rowIndex][colIndex] &&
									!queenOnGrid[rowIndex][colIndex]}
								style="grid-row: {rowIndex + 1}; grid-column: {colIndex + 1}; --color: {gridColor[
									(rowIndex + colIndex) % 2
								]};"
								onclick={() => toggleQueen(rowIndex, colIndex)}
								aria-label="Case {rowIndex + 1}, {colIndex + 1}"
								disabled={isCalculating}
							>
								{#if queenOnGrid[rowIndex][colIndex]}
									<div class="queen"></div>
								{/if}
							</button>
						{/each}
					</div>
				{/each}
			</div>

			<div class="button-container">
				<button class="reset-button" onclick={resetBoard} disabled={isCalculating}>Reset</button>
				<button class="solve-button" onclick={solveNQueens} disabled={isCalculating}>
					{#if isCalculating}
						<div class="spinner"></div>
						Calculating...
					{:else}
						Auto Solve
					{/if}
				</button>
			</div>
		</div>

		<div class="grid-controls-container">
			<h3>Controls</h3>

			<!-- Board size -->
			<div class="control-group">
				<label for="size">Board Size:</label>
				<input
					type="number"
					id="size"
					onchange={(e) => boardSizeChanged(parseInt((e.target as HTMLInputElement).value || '8'))}
					min="4"
					max="100"
					value={columns}
					disabled={isCalculating}
				/>
			</div>

			<!-- Statistics -->
			<div class="control-group">
				<h4>Statistics</h4>
				<div class="stats">
					<p>Queens placed: <span class="stat-value">{queenCount}</span></p>
					<p>Queens in conflict: <span class="stat-value conflict">{conflictCount}</span></p>
					<p class="success" class:visible={queenCount === rows && conflictCount === 0}>
						🎉 Valid solution!
					</p>
				</div>
			</div>

			<!-- Display options -->
			<div class="control-group">
				<h4>Display</h4>
				<label class="checkbox-label">
					<input type="checkbox" bind:checked={showControlledCells} />
					Show controlled squares
				</label>

				<label class="checkbox-label">
					<input type="checkbox" bind:checked={showConflictAlert} />
					Conflict alerts
				</label>
			</div>

			<!-- Board colors -->
			<div class="control-group">
				<h4>Colors</h4>
				<label for="color1">Light square:</label>
				<input type="color" id="color1" bind:value={gridColor[1]} />

				<label for="color2">Dark square:</label>
				<input type="color" id="color2" bind:value={gridColor[0]} />
			</div>

			<!-- Help -->
			<div class="control-group">
				<h4>Help</h4>
				<p class="help-text">
					Place {rows} queens on the board without them capturing each other.
				</p>
				<p class="help-text">A queen controls its row, column and diagonals.</p>
			</div>
		</div>
	</div>
</main>

<style lang="scss">
	@import '../lib/styles/variables.scss';

	h1 {
		color: $primary-color;
		text-align: center;
		margin-bottom: 20px;
	}

	main {
		min-height: 100vh;
		@include flex-center;
		flex-direction: column;
		background: #312e2b;
	}

	.global-container {
		display: flex;
		flex-direction: row;
	}

	.grid-controls-container {
		width: 25vw;
		height: 80vh;
		background: #f8f9fa;
		padding: 20px;
		overflow-y: auto;
		border-radius: 8px;
		margin-left: 20px;

		h3 {
			color: $primary-color;
			margin-bottom: 20px;
			text-align: center;
		}

		h4 {
			color: $text-color;
			margin: 15px 0 10px 0;
			font-size: 1.1rem;
		}

		.control-group {
			margin-bottom: 25px;
			padding-bottom: 15px;
			border-bottom: 1px solid #e9ecef;

			&:last-child {
				border-bottom: none;
			}

			label {
				display: block;
				margin-bottom: 5px;
				font-weight: 500;
				color: $text-color;
			}

			input[type='number'],
			input[type='color'] {
				width: 100%;
				padding: 8px;
				border: 1px solid #ddd;
				border-radius: 4px;
				font-size: 1rem;
				transition: opacity 0.3s ease;

				&:disabled {
					opacity: 0.6;
					cursor: not-allowed;
				}
			}

			input[type='color'] {
				height: 40px;
				margin-bottom: 10px;
				cursor: pointer;
			}

			.checkbox-label {
				display: flex;
				align-items: center;
				margin-bottom: 10px;
				cursor: pointer;

				input[type='checkbox'] {
					width: auto;
					margin-right: 8px;
				}
			}
		}

		.stats {
			p {
				margin: 8px 0;
				display: flex;
				justify-content: space-between;
				align-items: center;
			}

			.stat-value {
				font-weight: bold;
				padding: 2px 8px;
				border-radius: 12px;
				background-color: $primary-color;
				color: white;

				&.conflict {
					background-color: $accent-color;
				}
			}

			.success {
				color: $secondary-color;
				font-weight: bold;
				text-align: center;
				opacity: 0;
				transition: opacity 0.3s ease;

				&.visible {
					opacity: 1;
				}
			}
		}

		.help-text {
			font-size: 0.9rem;
			color: #666;
			line-height: 1.4;
			margin-bottom: 8px;
		}
	}

	.container {
		display: grid;
		height: 80vh;

		.row {
			display: contents;
		}

		.cell {
			background-color: var(--color);
			display: flex;
			align-items: center;
			justify-content: center;
			font-size: 1.5rem;
			color: $text-color;
			transition: background-color 0.3s ease;
			aspect-ratio: 1;
			border: none;
			cursor: pointer;
			position: relative;

			&:hover:not(:disabled) {
				opacity: 0.8;
			}

			&:disabled {
				cursor: not-allowed;
				opacity: 0.6;
			}

			&.controlled {
				position: relative;

				&::before {
					content: '';
					position: absolute;
					top: 0;
					left: 0;
					right: 0;
					bottom: 0;
					background-color: rgba(231, 76, 60, 0.3);
					pointer-events: none;
				}
			}

			.queen {
				background-image: url('/queen.png');
				background-size: cover;
				width: 100%;
				height: 100%;
				z-index: 2;
				position: relative;
			}
		}
	}

	.button-container {
		margin-top: 20px;
		display: flex;
		gap: 10px;
		justify-content: center;

		button {
			padding: 12px 24px;
			border: none;
			border-radius: 6px;
			cursor: pointer;
			font-size: 1rem;
			font-weight: 500;
			transition: all 0.3s ease;
			display: flex;
			align-items: center;
			gap: 8px;
			min-width: 120px;
			justify-content: center;

			&:disabled {
				cursor: not-allowed;
				opacity: 0.6;
				transform: none !important;
			}

			&.reset-button {
				background-color: $accent-color;
				color: white;

				&:hover:not(:disabled) {
					background-color: darken($accent-color, 10%);
					transform: translateY(-2px);
				}
			}

			&.solve-button {
				background-color: $secondary-color;
				color: white;

				&:hover:not(:disabled) {
					background-color: darken($secondary-color, 10%);
					transform: translateY(-2px);
				}
			}

			.spinner {
				width: 16px;
				height: 16px;
				border: 2px solid rgba(255, 255, 255, 0.3);
				border-top: 2px solid white;
				border-radius: 50%;
				animation: spin 1s linear infinite;
			}
		}
	}

	@keyframes spin {
		0% {
			transform: rotate(0deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}
</style>
