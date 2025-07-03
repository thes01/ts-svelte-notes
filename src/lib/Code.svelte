<script lang="ts">
	import * as lzstring from 'lz-string';
	import { createHighlighter } from 'shiki';
	import { escapeSvelte } from 'mdsvex';

	interface Props {
		code: string;
		lang?: 'ts';
	}

	const { code, lang = 'ts' }: Props = $props();

	const link = $derived(get_playground_url(code));

	function get_playground_url(code: string): string {
		// const compressed = lzstring.compressToEncodedURIComponent(code);
		const compressed = '';
		return `https://www.typescriptlang.org/play?#code/${compressed}`;
	}

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

{#await get_highlighed_code(code, lang) then code_html}
	{@html code_html}
{/await}

{#if lang === 'ts'}
	<a href={link} target="_blank">Link to TS Playground</a>
{/if}
