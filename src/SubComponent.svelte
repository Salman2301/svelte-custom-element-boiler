<script>

  import { createEventDispatcher } from "svelte";
  import { get_current_component } from "svelte/internal";

  const component = get_current_component();
  const svelteDispatch = createEventDispatcher();
  
  const dispatch = (name, detail) => {
    svelteDispatch(name, detail);
    component.dispatchEvent &&
      component.dispatchEvent(new CustomEvent(name, { detail }));
  };

  function handleClick() {
    dispatch("clickme2");
  }

</script>


<svelte:options tag="hello2-component" />

<button on:click={handleClick}>Click Me 2!</button>