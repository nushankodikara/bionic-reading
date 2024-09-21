<script lang="ts">
	import {
		Content,
		Grid,
		Row,
		Column,
		TextArea,
		Slider,
		Button,
		RadioButtonGroup,
		RadioButton
	} from 'carbon-components-svelte';
	import ArrowRight from 'carbon-icons-svelte/lib/ArrowRight.svelte';

	let input = '';
	let converted = '';
	let fixation = 3;
	let saccade = 1;
	let method = 'letters'; // Add a reactive variable for the selected method

	const convert = () => {
		const paragraphs = input.split('\n');
		const result = paragraphs.map((paragraph) => {
			const words = paragraph.split(/[ -]/);
			const processed = words
				.map((word, index) => {
					if (index % saccade !== 0) {
						return word;
					}
					const bfix = Math.floor((word.length * fixation) / 5);
					const letters = word.split('');
					if (letters.length < 2) {
						return `<span style="font-weight:bold;">${word}</span>`;
					}
					const bionic = letters.map((letter, i) => {
						if (i === bfix) {
							return '</span>' + letter;
						}
						return letter;
					});
					return '<span style="font-weight:bold;">' + bionic.join('');
				})
				.join(' ');
			return `<p>${processed}</p>`;
		});
		return result.join('<br />');
	};

	// Updated syllable conversion function
	const convertSyllables = () => {
		const paragraphs = input.split('\n');
		const result = paragraphs.map((paragraph) => {
			const syllables = paragraph.split(/(?=[aeiou])/gi); // Simple syllable splitting
			const processed = syllables
				.map((syllable, index) => {
					if (index % saccade !== 0) {
						return syllable;
					}
					const boldLength = Math.min(Math.ceil((syllable.length * fixation) / 5), syllable.length);
					const boldPart = syllable.substring(0, boldLength);
					const normalPart = syllable.substring(boldLength);
					return `<span style="font-weight:bold;">${boldPart}</span>${normalPart}`;
				})
				.join('');
			return `<p>${processed}</p>`;
		});
		return result.join('<br />');
	};

	// Update the conversion trigger to use the selected method
	const convertText = () => {
		if (method === 'letters') {
			converted = convert();
		} else {
			converted = convertSyllables();
		}
	};
</script>

<Content>
	<Grid>
		<Row>
			<Column>
				<h1>Convert Text to Bionic Reading</h1>
			</Column>
		</Row>
		<br />
		<Row padding>
			<Column lg padding>
				<div style="display:flex; flex-direction:column; gap: 1.5rem;">
					<h2>Input</h2>
					<TextArea labelText="Enter Your Text" placeholder="Lorem ipsume..." bind:value={input} />
					<h3>BR Algorithm</h3>
					<RadioButtonGroup
						bind:selected={method}
						legendText="Bionic Reading Algorithm"
						name="algorithm"
					>
						<RadioButton labelText="Letters" value="letters" />
						<RadioButton labelText="Syllables" value="syllables" />
					</RadioButtonGroup>
					<div style="display:flex; gap: 1rem;">
						<Slider labelText="Fixation" min={1} max={5} bind:value={fixation} />
						<Slider labelText="Saccade" min={1} max={5} bind:value={saccade} />
					</div>
					<Button kind="secondary" icon={ArrowRight} on:click={convertText}>Convert</Button>
				</div>
			</Column>
			<Column lg padding>
				<h2>Result</h2>
				<br />
				<p>
					{@html converted}
				</p>
			</Column>
		</Row>
	</Grid>
</Content>
