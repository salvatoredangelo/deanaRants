<script context="module">
  import client from '../sanityClient'
  export function preload({ params, query }) {
    return client
      .fetch(
        '*[_type == "author"]{name, "image": image.asset->url, "alt": image.alt, "bio": bio[0].children[0].text}',
      )
      .then((author) => {
        return { author }
      })
      .catch((err) => this.error(500, err))
  }
</script>

<script>
  import { slide, blur } from 'svelte/transition'
  export let author
  author = author[0]
  const { name, image, alt, bio } = author
</script>

<style>
  img {
    width: 100%;
    height: 10rem;
    object-fit: cover;
    filter: brightness(50%);
  }
  p {
    color: var(--black);
  }
</style>

<svelte:head>
  <title>About Deana</title>
</svelte:head>

<h1>About {name}</h1>
<img
  in:slide={{ duration: 1000 }}
  src={`${image}?fit=crop&crop=entropy`}
  {alt} />
<p in:blur={{ delay: 1000 }}>{bio}</p>
