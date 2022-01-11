<script lang="ts">
	import TestimonialCard from "./components/TestimonialCard.svelte"
	import TestimonialGrid from "./components/TestimonialGrid.svelte"

	let testimonialsPromise: Promise<any>

	const fetchUsers = async () => {
			const data = await fetch("https://api.jsonbin.io/v3/b/61db78c139a33573b326338e", {
				"headers": {
					"X-Master-Key": "$2b$10$xix0xxs89s7XPhfaMgqu0uYcdtT8NwZ9UShzsQU5o2KA17WfT6f2a"
				}
			})
			
			const response = await data.json()
			return await response
		}

	testimonialsPromise = fetchUsers()
</script>

<main>
	<!-- svelte-ignore empty-block -->
	{#await testimonialsPromise}
	{:then data}
		<TestimonialGrid fetchedData={data.record} />
	{/await}
</main>
