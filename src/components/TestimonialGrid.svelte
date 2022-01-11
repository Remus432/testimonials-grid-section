<script lang="ts">
  export let fetchedData: UsersData

  import TestimonialCard from "./TestimonialCard.svelte"
  import VanillaTilt from "vanilla-tilt"
  import { onMount } from "svelte"
  import Ajv from "ajv"

	const ajv = new Ajv()

  interface UsersData {
    users: []
  }

  let errMsg: string
  let isErr: boolean = false
  let windowW: number = window.innerWidth

  const schema = {
		type: "object",
		properties: {
			users: { 
				type: "array", 
        minItems: 5,
				nullable: true
			}
		},
		additionalProperties: false
	}

  const validate = ajv.compile(schema)

  if (!validate(fetchedData)) {
    isErr = true
    throw validate.errors
  } 

  onMount(() => {
    window.addEventListener("resize", () => windowW = window.innerWidth)
    const cards: NodeListOf<Element> = document.querySelectorAll(".testimonial__card")

    if (windowW >= 1300) {
      cards.forEach((card: HTMLElement) => VanillaTilt.init(card))
    }
  })
</script>

<div class="testimonial__grid">
  {#if !isErr}
    {#each fetchedData.users as user, index}
      <TestimonialCard user={user} index={index+1} />
    {/each}
  {:else}
    <p>{errMsg}</p>
  {/if}
</div>

<style lang="scss">
  @use "../styles/query";

  .testimonial__grid {
    padding: 7.1rem 2.4rem;
    display: inline-grid;
    grid-gap: 2.4rem;
    grid-template-areas:
      "card-1"
      "card-2" 
      "card-3"
      "card-4"
      "card-5";

    @include query.respond(tab) {
      grid-template-areas:
        "card-1 card-1 card-2 card-2"
        "card-3 card-3 card-4 card-4"
        "card-5 card-5 card-5 card-5"
    }

    @include query.respond(desktop) {
      padding: 5rem 16.5rem;      
      grid-gap: 3rem;
      grid-template-areas:
        "card-1 card-1 card-1 card-2 card-5"
        "card-3 card-4 card-4 card-4 card-5"
    }
  }
</style>