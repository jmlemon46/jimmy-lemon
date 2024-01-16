<script>
	import { siteTitle } from '$lib/config';
	import { onMount } from 'svelte';
	import { formatDate } from '$lib/assets/js/dateUtils.js';
	let data;

	async function fetchData() {
		const response = await fetch('/api/posts.json');
		if (response.ok) {
			let rawData = await response.json();
			// Format each date
			data = rawData.map((post) => ({
				...post,
				date: formatDate(post.date)
			}));
		} else {
			console.error('Error fetching data');
		}
	}

	onMount(() => {
		fetchData();
	});
</script>

<svelte:head>
	<title>{siteTitle}</title>
</svelte:head>

<section class="hero">
	<h1>
		Hey, I'm <strong>Jimmy</strong>.<br />
		I'm just here to <strong>talk about life, leadership</strong> and the pursuit of
		<strong>the perfect dad joke.</strong>
	</h1>
</section>

<section class="split-section">
	<div class="socialLinks">
		<img src="/images/jimmyheadshot.jpg" alt="Follow Jimmy on social media" class="headshot" />
    <h2>Connect with me.</h2>
		<ul class="social-buttons">
			<li>
				<a href="https://www.linkedin.com/in/jimmylemon/" target="_blank"
					><img src="/images/linkedin@2x.png" alt="linkedIn Link" />@jimmylemon</a>
			</li>
			<li>
				<a href="https://www.instagram.com/_jimmylemon/" target="_blank"
					><img src="/images/instagram@2x.png" alt="linstagram Link" /><span>@_jimmylemon</span></a
				>
			</li>
			<li>
				<a href="https://www.threads.net/@_jimmylemon" target="_blank"
					><img src="/images/threads@2x.png" alt="threads Link" /><span>@_jimmylemon</span></a
				>
			</li>
			<li>
				<a href="https://www.facebook.com/jimmy.lemon" target="_blank"
					><img src="/images/facebook@2x.png" alt="facebook Link" /><span>/jimmy.lemon</span></a
				>
			</li>

      <li style="grid-column: span 2;">
				<a href="https://bullseyetotalmedia.com" target="_blank"
					><img src="/images/btm-icon-round.png" alt="facebook Link" /><span><strong>work with me:</strong> bullseyetotalmedia.com</span></a
				>
			</li>
		</ul>
	</div>
	<div class="latest-post">
		<h2>The latest from the blog.</h2>
		{#if data && data.length > 0}
			<ul class="post-list">
				{#each data.slice(0, 4) as item}
					<a href="/blog/{item.slug}">
						<li>
							<img src="{item.coverImage}" class="home-thumb" alt="{item.excerpt}">
							<div>
								<h4>{item.title}</h4>
								<span class="blog-date">{item.date}</span>
								<p class="excerpt">{item.excerpt}</p>
							</div>
						</li>
					</a>
				{/each}
			</ul>
		{:else}
			<p>Loading...</p>
		{/if}

		<a href="/blog" class="btn">More from the Blog</a>
	</div>
</section>

<style>
	section.hero {
		background-color: var(--yellow);
		min-height: 60vh;
		display: flex;
		align-items: center;
		border-bottom: 2px solid var(--black);
	}

	section.hero h1 {
		font-weight: 300;
		max-width: 55vw;
		font-weight: 300;
		margin: 0 10vw;
	}

	@media screen and (max-width:1040px) {
		section.hero h1 {
			max-width:unset;
		}
		section.hero {
			min-height:unset;
			padding:4rem 0;
		}
	}

	.split-section {
		display: grid;
		grid-template-columns: 1fr 1fr;
	}

	@media screen and (max-width:1040px) {
		.split-section {
			grid-template-columns: 1fr;
			padding:4rem 0;
		}
	}

	.split-section > div {
		padding: 5vw;
	}

	.split-section > div:first-of-type {
		border-right: 2px solid var(--black);
	}

	h2 {
		margin-bottom: 2rem;
	}
	h4 {
		margin-bottom: 0px;
		font-weight: 300;
	}

	span {
		margin-top: 0px;
	}

	.latest-post ul {
		padding-left: 0px;
	}
	li {
		list-style-type: none;
	}
	.latest-post li {
		border: 1px solid var(--black);
		padding: 1rem;
		list-style-type: none;
		transition: all 0.3s ease;
		margin-bottom: 1rem;
		background: #fff;
		display:grid;
		grid-template-columns:1fr 3fr;
		gap: 1rem;
	}

	.latest-post img {
		aspect-ratio: 1 / 1.2;
		object-fit: cover;
	}

	.latest-post a:hover li {
		border-left: 20px solid var(--black);
    cursor:pointer;
	}

	a {
		text-decoration: none;
	}

	.socialLinks img {
		max-height: 50px;
		width: auto;
		display: inline;
	}

	.socialLinks img.headshot {
		margin: 0 auto;
		max-height: 150px;
		border-radius: 100%;
}
ul.social-buttons {
	padding-left:0px;
}

  .socialLinks {
    display: flex;
    flex-direction: column;
  }

  .socialLinks ul {
    display:grid;
    grid-template-columns: 1fr 1fr;
    gap:1rem;
  }

  .socialLinks li a {
    display:flex;
    align-items: center;
    background:#fff;
    padding:.5rem;
    gap:1rem;
    transition: all .3s ease;
    border-left:0px solid var(--black);
      }

	  @media screen and (max-width:800px) {
		.socialLinks li a {
			font-size:.88rem;
		}

		.socialLinks img {
		max-height: 40px;
	  }
	}

      .socialLinks li a:hover {
        border-left:8px solid var(--black);
        padding-left:12px;
        cursor:pointer;
      }

      .socialLinks h2 {
        text-align: center;
        margin-top:1rem;
      }

	  .post-list a:hover .excerpt {
		opacity:1000;
		max-height:500px;
	  }

	  .post-list p {
		margin-bottom:0px;
	  }
	  .excerpt {
		font-size:1rem;
		transition: .5s ease;
		opacity:0;
		max-height:0px;
		line-height:1.2rem;
	  }

	  .blog-date {
		font-size:1.1rem;
	  }
</style>
