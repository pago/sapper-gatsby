<script context="module">
import { fetchQuery } from '@myapp/graphql';

export async function preload({ params, query }) {
	const response = await fetchQuery(this, {
			query: `query findBlogPosts {
				allMarkdownRemark {
					nodes {
						frontmatter {
							title
							slug
							author
						}
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
					}
					totalCount
				}
			}`
	});
	const { data } = await response.json();

	return {
		total: data.allMarkdownRemark.totalCount,
		posts: data.allMarkdownRemark.nodes.map(post => ({
			title: post.frontmatter.title,
			author: post.frontmatter.author,
			slug: post.frontmatter.slug,
			excerpt: post.excerpt,
			timeToRead: post.timeToRead,
			wordCount: post.wordCount.words,
		}))
	};
}
</script>

<script>
	export let posts;
</script>

<style>
	ul {
		margin: 0 0 1em 0;
		line-height: 1.5;
	}
</style>

<svelte:head>
	<title>Blog</title>
</svelte:head>

<h1>Recent posts</h1>

<ul>
	{#each posts as post}
		<!-- we're using the non-standard `rel=prefetch` attribute to
				tell Sapper to load the data for the page as soon as
				the user hovers over the link or taps it, instead of
				waiting for the 'click' event -->
		<li><a rel='prefetch' href='blog/{post.slug}'>{post.title}</a></li>
	{/each}
</ul>