<script>
  import Info from "./Info.svelte";
  import Nested from "./Nested.svelte";

  async function getRandomNumber() {
    const res = await fetch(`https://svelte.dev/tutorial/random-number`);
    const text = await res.text();

    if (res.ok) {
      return text;
    } else {
      throw new Error(text);
    }
  }

  let mousePosition = { x: 0, y: 0}

  const handleMouseMovement = (event) => {
	  mousePosition.x = event.clientX;
	  mousePosition.y = event.clientY;
  }

  let promise = getRandomNumber();

  const handleClick = () => {
    promise = getRandomNumber();
  };

  let user = { loggedIn: false };

  const toggle = () => {
    user.loggedIn = !user.loggedIn;
  };

  let numbers = [1, 2, 3, 4];

  function addNumber() {
    numbers = [...numbers, numbers.length + 1];
  }

  $: sum = numbers.reduce((t, n) => t + n, 0);

  const pkg = {
    name: "svelte",
    version: 3,
    speed: "blazing",
    website: "https://svelte.dev",
  };

  let cats = [
    { id: "J---aiyznGQ", name: "Keyboard Cat" },
    { id: "z_AbfPXTKms", name: "Maru" },
    { id: "OUtn3pvWmpg", name: "Henri The Existential Cat" },
  ];
</script>

<main on:mousemove={handleMouseMovement}>
  <p>{numbers.join(" + ")} = {sum}</p>
  <button on:click={addNumber}> Add a number; </button>
  <Nested answer={42} />
  <Nested />
  <Info {...pkg} />
  {#if !user.loggedIn}
    <button on:click={toggle}>Log In</button>
  {:else}
    <button on:click={toggle}>Log Out</button>
  {/if}

  <h1>The Famous Cats of Youtube</h1>

  <ul>
    {#each cats as { id, name }, idx}
      <li>
        <a target="_blank" href="https://youtube.com/watch?v={id}">
          {idx + 1}: {name}
        </a>
      </li>
    {/each}
  </ul>

  <button on:click={handleClick}>Generate random number</button>
  {#await promise}
    <p>...waiting</p>
  {:then number}
    <p>The number is {number}</p>
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}

  <div>
	Mouse Position: {mousePosition.x}, {mousePosition.y}
</div>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  button {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
