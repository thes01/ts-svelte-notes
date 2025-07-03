<script lang="ts">
	import * as lzstring from 'lz-string';
	import { createHighlighter } from 'shiki';
	import { escapeSvelte } from 'mdsvex';

	interface Props {
		code: string;
	}

	const { code }: Props = $props();

	function get_playground_url(code: string): string {
		const compressed = lzstring.compressToEncodedURIComponent(code);
		return `https://www.typescriptlang.org/play?#code/${compressed}`;
	}

	let link = $derived(get_playground_url(code));

	async function get_highlighed_code(code: string, lang: string) {
		const theme = 'github-dark';
		const highlighter = await createHighlighter({
			themes: [theme],
			langs: ['javascript', 'typescript']
		});
		const html = escapeSvelte(highlighter.codeToHtml(code, { lang, theme }));

		return html;
	}
</script>

{#await get_highlighed_code(code, 'ts') then code_html}
	{@html code_html}
{/await}

<a href={link} target="_blank">Link to TS Playground</a>
