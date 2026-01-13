<script>
  import { getContext } from "svelte";
  import JSONTree from "./JSONTree.svelte";

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  export let value = {};
  export let showKeyCount = false;
  export let quiet = false;
  export let monospace = false;

  const safeParse = (value) => {
    try {
      const result = typeof value === "string" ? JSON.parse(value) : value;
      return result;
    } catch (e) {
      return value;
    }
  };
  $: parsedData = safeParse(value ?? {});
  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles?.normal,
      overflow: "auto",
    },
  };
</script>

<div use:styleable={$component.styles}>
  <JSONTree value={parsedData} {showKeyCount} {quiet} {monospace} />
</div>
