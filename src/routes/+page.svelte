<script>
	import { onMount } from 'svelte';

	let count = 50;
	let activeSection = '';

	let y = 0;
	let lastY = 0;
	let sectionElement;

	// Function to observe sections
	function observeSections(node) {
		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						history.replaceState(null, null, `#${entry.target.id}`);
						activeSection = entry.target.id;
					}
				});
			},
			{
				rootMargin: '0px 0px -100% 0px',
				threshold: 0
			}
		);

		observer.observe(node);

		return {
			destroy() {
				observer.unobserve(node);
			}
		};
	}

	onMount(() => {
		sectionElement.addEventListener('scroll', function (e) {
			alert('The scroll event only triggers when there is content to scroll.');
		});
	});

	const scrollToTheater = (y) => {
		let dy = lastY - y;
		lastY = y;

		// let bottom = sectionElement.getBoundingClientRect().bottom;
		// alert(bottom);

		if (dy < -10) {
			document.body.parentNode.scrollTo({
				top: sectionElement.offsetTop,
				left: 0,
				behavior: 'smooth'
			});
		}
	};

	$: scrollToTheater(y);
</script>

<svelte:window bind:scrollY={y} />

<div id="container" class="flex flex-col bg-gray-100 m-auto rounded-xl p-4 py-2 w-4/5">
	<h1>Lorems</h1>

	{#each Array.from({ length: count }, (_, i) => i + 1) as index}
		<section
			id="lorem-{index}"
			class={activeSection === `lorem-${index}` ? 'active' : ''}
			use:observeSections
			bind:this={sectionElement}
		>
			<h2 class="bg-[#ff0000] rounded p-2 mt-2">Lorem {index}</h2>
			<p>
				Lorem ipsum dolor sit amet consectetur adipisicing elit. Quo explicabo dolorem nobis at
				sapiente fugiat excepturi, enim velit, ipsum dolore reprehenderit eum vitae dolor
				recusandae, in repudiandae! Nesciunt, ratione eius. Exercitationem pariatur obcaecati fugit
				ea temporibus assumenda accusamus, inventore sed dolores quam facere doloremque nisi dolorum
				repudiandae id cum vitae excepturi nulla minima quod molestiae rem dolor! Dolorem, labore.
				Maxime. Reiciendis nostrum error voluptas magnam modi doloremque quisquam. Omnis dolore,
				quae perspiciatis, laboriosam obcaecati quam veniam dolorum distinctio laudantium dolores
				molestias, recusandae temporibus at impedit sequi? Placeat voluptate soluta hic.
			</p>
		</section>
	{/each}
</div>

<style>
	section:not(.active) h2 {
		@apply from-black;
	}

	.active h2 {
		@apply sticky top-2;
		background: linear-gradient(red, 80%, #ff00000a);
	}
</style>
