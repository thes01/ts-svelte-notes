<script lang="ts">
	import { createHighlighter } from 'shiki';
	import { escapeSvelte } from 'mdsvex';

	interface Props {
		code: string;
		lang?: 'ts';
	}

	const { code, lang = 'ts' }: Props = $props();

	const link_promise = $derived(get_playground_url(code));

	async function get_playground_url(code: string) {
		// For some reason this doesn't work in the SSR.
		const lzstring = await import('lz-string');
		const compressed = lzstring.compressToEncodedURIComponent(code);
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
	{#await link_promise then link}
		<a href={link} target="_blank">Link to TS Playground</a>
	{/await}
{/if}
