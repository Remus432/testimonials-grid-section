<script lang="ts">
  export let user
  export let index:number

  import Ajv, { JSONSchemaType } from "ajv"
  import { onMount } from "svelte";
  import gsap from "gsap"

	const ajv = new Ajv()

  const theme = (index === 1 || index === 2 || index === 4) ? "light" : "dark"
  
  interface TestimonialUser {
		picture: string,
		name: string,
		isVerified: boolean,
		testimonial_summary: string,
		testimonial_extended: string
	} 


  const schema: JSONSchemaType<TestimonialUser> = {
		type: "object",
		properties: {
      picture: { type: "string" },
      name: { type: "string" },
      isVerified: { type: "boolean" },
      testimonial_summary: { type: "string" },
      testimonial_extended: { type: "string" }
		},
    required: ["picture", "name", "isVerified", "testimonial_summary", "testimonial_extended"],
		additionalProperties: false
	}

  const validate = ajv.compile(schema)

  if (!validate(user)) throw validate.errors

  onMount(() => {
    gsap.to(".testimonial__card--1", {
      opacity: 1,
      "clip-path": "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
      ease: "power2.out",
      duration: 1
    })
    gsap.to(".testimonial__card--3", {
      opacity: 1,
      "clip-path": "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
      ease: "power2.out",
      delay: .3,
      duration: 1
    })
    gsap.to(".testimonial__card--2", {
      opacity: 1,
      "clip-path": "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
      ease: "power2.out",
      delay: .6,
      duration: 1
    })
    gsap.to(".testimonial__card--5", {
      opacity: 1,
      "clip-path": "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
      ease: "power2.out",
      delay: .9,
      duration: 1
    })
    gsap.to(".testimonial__card--4", {
      opacity: 1,
      "clip-path": "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
      ease: "power2.out",
      delay: 1.2,
      duration: 1
    })
  })

</script>

<article 
  class="testimonial__card testimonial__card--{index} {theme}" 
  data-tilt-glare 
  data-tilt-max-glare="0.5"
  data-tilt-scale="1.05">
  <div class="testimonial__user">
    <img src={user.picture} class="testimonial__user-avatar" alt={user.name} />
    <div class="testimonial__user-info">
      <p class="testimonial__user-name">{user.name}</p>
      {#if user.isVerified}
        <span class="testimonial__user-status">Verified Graduate</span>
      {/if}
    </div>
  </div>
  <div class="testimonial__content">
    <h2 class="testimonial__summary">{user.testimonial_summary}</h2>
    <p class="testimonial__extended">{user.testimonial_extended}</p>
  </div>
</article>

<style lang="scss">
  .testimonial__card {
    border-radius: 8px;
    grid-row: min-content;
    padding: 2.6rem 3.2rem 3.2rem 3.2rem;
    position: relative;
    transform-style: preserve-3d;
    transform: perspective(100rem);
    opacity: 0;

    & > * {
      transform: translateZ(5rem);
    }

    @for $i from 1 through 5 {
      &--#{$i} {
        grid-area: card-#{$i};
      }
    }

    @for $i from 3 through 5 {
      &--#{$i} {
        .testimonial__user { margin-bottom: 2.4rem };
      }
    }

    &.light {
      & .testimonial__user { 
        &-name { color: #fff; }
        &-status { color: #fff; } 
      }

      & .testimonial__summary { color: #fff; }

      & .testimonial__extended { color: #fff; }
    }

    &.dark {
      & .testimonial__user { 
        &-name { color: #48556A; }
        &-status { color: #48556A; }
      }

      & .testimonial__summary { color: #48556A; }

      & .testimonial__extended { color: #48556A; }
    }

    &--3,
    &--5 {
      box-shadow: 4rem 6rem 5rem -4.7rem rgba($color: #48556A, $alpha: .24);
    }

    &--1 {
      background-color: hsl(263, 55%, 52%);
      z-index: 10;

      & .testimonial__user-avatar {
        outline: 3px solid #A775F1;
      }
    }

    &--2 {
      background-color: hsl(217, 19%, 35%);
    }

    &--3 {
      background-color: hsl(0, 0%, 100%);
    }

    &--4 {
      background-color: hsl(219, 29%, 14%);

      & .testimonial__user-avatar {
        outline: 3px solid #A775F1;
      }
    }

    &--5 {
      background-color: hsl(0, 0%, 100%);
    }
  }

  .testimonial__user {
    align-items: center;
    display: flex;
    margin-bottom: 1.8rem;

    &-avatar {
      border-radius: 50%;
      margin-right: 1.7rem;
      width: 3.2rem;
    }

    &-name {
      font-size: 1.3rem;
      font-weight: 500;
    }

    &-status {
      font-size: 1.1rem;
      opacity: .5;
    }
  }

  .testimonial__summary {
    font-size: 2rem;
    margin-bottom: 1.6rem;
  }

  .testimonial__extended {
    opacity: .7;
    line-height: 1.8rem;
  }
</style>