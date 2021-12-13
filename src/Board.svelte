<script>
	export let easy;
	let board = [];

	let neq_indices = [];		// these are the index values that each index cannot be equal to

	for (let i = 0; i < 81; i++) {
		let indices = new Set();

		// row limitations
		let s = 9 - (i % 9);
		for (let k = s - 9; k < s; k++) {
			if (i != i + k) {
				indices.add(i + k);
			}
		}

		// column limitations
		for (let k = -72; k < 81; k += 9) {
			if (i + k >= 0 && i + k < 81 && i + k != i) {
				indices.add(i + k);
			}
		}

		// 3x3 limitations
		let base = [0, 1, 2, 9, 10, 11, 18, 19, 20];
		let found = false;
		let m = 0;
		for (let m = 0; m < 3; m++) {			// row
			for (let n = 0; n < 3; n++) {		// column
				found = base[i] + (27 * n) + (3 * m) == i;
				if (found) {
					for (let b of base) {
						let r = (b + (27 * n) + (3 * m));
						indices.add(r);
					}
				break;
				}
			}
			if (found) {
				break;
			}
		}
		neq_indices.push(indices);
	}

	for (let i = 0; i < 81; i++) {
		let neq = [];
		let x_i = Math.floor(Math.random() * 9 + 1);
		for (let value of neq_indices[i]) {
			if (board[value] == x_i) {
				x_i = Math.floor(Math.random() * 9 + 1);
			}
		}
		board.push(x_i);
	}

	let upperLimit;
	let blanks = [];
	if (easy) {
		upperLimit = 20;
	}
	else {
		upperLimit = 40;
	}
	for (let k = 0; k < upperLimit; k++) {
		blanks.push(Math.floor(Math.random() * 81 + 1));
	}
</script>

<div>
	<h1>sudoku</h1>
	<table>
		{#each {length: 9} as _, i}
			<tr>
				{#each {length: 9} as _, j}
					<td>
						{#if blanks.includes(9 * i + j)}
							<input>
						{:else}
							{board[9 * i + j]}
						{/if}
					</td>
				{/each}
			</tr>
		{/each}
	</table>
</div>

<style>
	table {
		width: 80vh;
		height: 80vh;
	}
	table, tr, td {
		border: solid;
	}
	input, td {
		width: 20px;
	}
</style>
