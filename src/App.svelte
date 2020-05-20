<script>
  import SubComponent from "./SubComponent.svelte";
  // import { createEventDispatcher } from "svelte";
  // const dispatch =createEventDispatcher();
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
    console.log("clicked!");
    dispatch("clickme");
  }

</script>


<svelte:options tag="hello-component" />

<button on:click={handleClick}>Click Me!</button>
<SubComponent on:clickme2={handleClick}></SubComponent>