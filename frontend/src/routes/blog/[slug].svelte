<script context="module">
import { fetchQuery } from '@myapp/graphql';

export async function preload({ params, query }) {
	// the `slug` parameter is available because
	// this file is called [slug].svelte
	// const res = await this.fetch(`blog/${params.slug}.json`);

	const gql = `
		query findPage($slug: String!) {
			allMarkdownRemark(filter: {frontmatter: {slug: {eq: $slug}}}) {
				nodes {
					html
					htmlAst
					rawMarkdownBody
					tableOfContents
					timeToRead
					excerpt
					wordCount {
						paragraphs
						sentences
						words
					}
					fileAbsolutePath
					frontmatter {
						title
						slug
						author
					}
				}
			}
		}
	`;

	const res = await fetchQuery(this, {
		query: gql,
		variables: {
			slug: params.slug
		}
	});

	const { data } = await res.json();

	if (res.status === 200) {
		return {
			post: data.allMarkdownRemark.nodes.map(post => ({
				title: post.frontmatter.title,
				author: post.frontmatter.author,
				slug: post.frontmatter.slug,
				excerpt: post.excerpt,
				timeToRead: post.timeToRead,
				wordCount: post.wordCount.words,
				html: post.html
			}))[0]
		};
	} else {
		this.error(res.status, data.message);
	}
}
</script>

<script>
	export let post;
</script>

<style>
	/*
		By default, CSS is locally scoped to the component,
		and any unused styles are dead-code-eliminated.
		In this page, Svelte can't know which elements are
		going to appear inside the {{{post.html}}} block,
		so we have to use the :global(...) modifier to target
		all elements inside .content
	*/
	.content :global(h2) {
		font-size: 1.4em;
		font-weight: 500;
	}

	.content :global(pre) {
		background-color: #f9f9f9;
		box-shadow: inset 1px 1px 5px rgba(0,0,0,0.05);
		padding: 0.5em;
		border-radius: 2px;
		overflow-x: auto;
	}

	.content :global(pre) :global(code) {
		background-color: transparent;
		padding: 0;
	}

	.content :global(ul) {
		line-height: 1.5;
	}

	.content :global(li) {
		margin: 0 0 0.5em 0;
	}
</style>

<svelte:head>
	<title>{post.title}</title>
</svelte:head>

<h1>{post.title}</h1>

<div class='content'>
	{@html post.html}
</div>
