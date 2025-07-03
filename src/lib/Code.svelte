<script lang="ts">
	import { onMount, type Snippet } from 'svelte';
	import * as lzstring from 'lz-string';

	interface Props {
		children?: Snippet;
	}

	const { children }: Props = $props();

	let code_elem: HTMLDivElement;

	let link = $state('');

	function generatePlaygroundURL(code: string): string {
		const compressed = lzstring.compressToEncodedURIComponent(code);
		return `https://www.typescriptlang.org/play?#code/${compressed}`;
	}

	function update_playground_link() {
		const code = code_elem.innerText;
		link = generatePlaygroundURL(code);
	}

	onMount(() => {
		update_playground_link();
	});
</script>

<div bind:this={code_elem}>
	{@render children?.()}
</div>

<a href={link} target="_blank">Link to TS Playground</a>
