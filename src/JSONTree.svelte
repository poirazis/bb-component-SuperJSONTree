<script>
  import { writable } from "svelte/store";

  export let value = {};
  export let showKeyCount = false;
  export let quiet = false;
  export let monospace = false;

  const expandedNodes = writable([]);

  function toggleNode(key) {
    try {
      expandedNodes.update((nodes) => {
        const index = nodes.indexOf(key);
        if (index === -1) {
          return [...nodes, key];
        } else {
          return nodes.filter((node) => node !== key);
        }
      });
    } catch (e) {
      console.error("Error toggling node:", e);
    }
  }

  function hasChildren(value) {
    return (
      (typeof value === "object" && value !== null && !Array.isArray(value)) ||
      Array.isArray(value)
    );
  }

  function isDateString(value) {
    if (typeof value !== "string") return false;
    const isoDateRegex = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}.*Z$/;
    if (isoDateRegex.test(value)) {
      const date = new Date(value);
      return !isNaN(date.getTime());
    }
    return false;
  }

  function getTypeClass(value) {
    if (value === null) return "null";
    if (isDateString(value)) return "date";
    if (
      typeof value === "object" &&
      !Array.isArray(value) &&
      Object.keys(value).length === 0
    )
      return "null";
    if (Array.isArray(value) && value.length === 0) return "null";
    switch (typeof value) {
      case "string":
        return "string";
      case "number":
        return "number";
      case "boolean":
        return "boolean";
      case "undefined":
        return "undefined";
      default:
        return "other";
    }
  }

  function renderNode(key, value, path = "") {
    const fullPath = path ? `${path}.${key}` : key;
    const isObject = typeof value === "object" && value !== null;
    const isArray = Array.isArray(value);
    const hasChildNodes =
      hasChildren(value) &&
      !(
        (isObject && !isArray && Object.keys(value).length === 0) ||
        (isArray && value.length === 0)
      );
    const typeClass = getTypeClass(value);
    const itemCount = isArray
      ? value.length
      : isObject && !isArray
        ? Object.keys(value).length
        : 0;
    const countLabel = showKeyCount
      ? isArray
        ? `[] ${itemCount} items`
        : isObject && !isArray
          ? `{} ${itemCount} Keys`
          : ""
      : "";

    return {
      key,
      value,
      fullPath,
      isObject,
      isArray,
      hasChildNodes,
      typeClass,
      countLabel,
    };
  }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
  class="tree"
  class:quiet
  style:font-family={monospace ? "monospace" : "inherit"}
  style:font-size={monospace ? "11px" : "13px"}
>
  {#if !value || Object.keys(value).length === 0}
    <p>{"{ }"}</p>
  {:else}
    <ul class="list">
      {#each Object.entries(value) as [key, value]}
        {@const node = renderNode(key, value)}
        <li class="list-item">
          <div class="node">
            {#if node.hasChildNodes}
              <button class="toggle" on:click={() => toggleNode(node.fullPath)}>
                {#if $expandedNodes.includes(node.fullPath)}
                  <i class="ri-arrow-drop-down-line"></i>
                {:else}
                  <i class="ri-arrow-drop-right-line"></i>
                {/if}
              </button>
            {:else}
              <span class="spacer"></span>
            {/if}

            <span
              class="key"
              on:click={() => node.hasChildNodes && toggleNode(node.fullPath)}
              >{node.key}: {#if node.countLabel}<span class="count"
                  >{node.countLabel}</span
                >{/if}</span
            >
            {#if !node.hasChildNodes}
              <span class="value {node.typeClass}"
                >{JSON.stringify(node.value)}</span
              >
            {/if}
          </div>

          {#if node.hasChildNodes && $expandedNodes.includes(node.fullPath)}
            <ul class="list nested">
              {#if node.isArray}
                {#each node.value as item, index}
                  {@const childNode = renderNode(
                    String(index),
                    item,
                    node.fullPath
                  )}
                  <li class="list-item nested-item">
                    <div class="node">
                      {#if childNode.hasChildNodes}
                        <button
                          class="toggle"
                          on:click={() => toggleNode(childNode.fullPath)}
                        >
                          {#if $expandedNodes.includes(childNode.fullPath)}
                            <i class="ri-arrow-drop-down-line"></i>
                          {:else}
                            <i class="ri-arrow-drop-right-line"></i>
                          {/if}
                        </button>
                      {:else}
                        <span class="spacer"></span>
                      {/if}
                      <!-- svelte-ignore a11y-click-events-have-key-events -->
                      <span
                        class="key"
                        on:click={() =>
                          childNode.hasChildNodes &&
                          toggleNode(childNode.fullPath)}
                        >{childNode.key}: {#if childNode.countLabel}<span
                            class="count">{childNode.countLabel}</span
                          >{/if}</span
                      >
                      {#if !childNode.hasChildNodes}
                        <span class="value {childNode.typeClass}"
                          >{JSON.stringify(childNode.value)}</span
                        >
                      {/if}
                    </div>
                    {#if childNode.hasChildNodes && $expandedNodes.includes(childNode.fullPath)}
                      <ul class="list nested">
                        <li class="list-item nested-item">
                          <svelte:self
                            value={childNode.value}
                            {showKeyCount}
                            {quiet}
                            {monospace}
                          />
                        </li>
                      </ul>
                    {/if}
                  </li>
                {/each}
              {:else if node.isObject}
                <li class="list-item nested-item">
                  <svelte:self
                    value={node.value}
                    {showKeyCount}
                    {quiet}
                    {monospace}
                  />
                </li>
              {/if}
            </ul>
          {/if}
        </li>
      {/each}
    </ul>
  {/if}
</div>

<style>
  .tree {
    line-height: 1.65rem;
    font-size: 13px;
    color: var(--spectrum-global-color-gray-700);
    --text-color: var(--spectrum-global-color-gray-200);
    --string-color: var(--spectrum-global-color-blue-400);
    --number-color: var(--spectrum-global-color-red-400);
    --boolean-color: var(--spectrum-global-color-purple-400);
    --null-color: var(--spectrum-global-color-green-400);
    --date-color: var(--spectrum-global-color-orange-400);
    --toggle-color: var(--spectrum-global-color-gray-700);
    --spacer-color: var(--spectrum-global-color-gray-500);
    --empty-text-color: var(--spectrum-global-color-purple-500);
  }

  .tree.quiet {
    --string-color: var(--spectrum-global-color-gray-600);
    --number-color: var(--spectrum-global-color-gray-600);
    --boolean-color: var(--spectrum-global-color-gray-600);
    --null-color: var(--spectrum-global-color-gray-600);
    --date-color: var(--spectrum-global-color-gray-600);
  }

  .list {
    list-style: none;
    padding-left: 0;
    padding-right: 0.5rem;
    margin: 0;
    transition: all 0.2s ease-in-out;
  }

  .list-item {
    margin-left: 0;
  }

  .node:hover {
    background-color: var(--spectrum-global-color-gray-100);
  }

  .nested {
    padding-left: 18px;
  }

  .nested-item {
    margin-left: 0;
  }

  .node {
    display: flex;
    align-items: center;
    gap: 0.25rem;
    box-sizing: border-box;
  }

  .toggle {
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;
    width: 14px;
    text-align: center;
    color: var(--toggle-color);
  }

  .spacer {
    min-width: 14px;
    text-align: center;
    color: var(--spacer-color);
  }

  .key {
    cursor: pointer;
    max-width: 7rem;
    margin-right: 0.5rem;
  }

  .count {
    color: var(--spectrum-global-color-gray-500);
    font-family: monospace;
  }

  .value {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }

  .value.string {
    color: var(--string-color);
  }

  .value.number {
    color: var(--number-color);
  }

  .value.boolean {
    color: var(--boolean-color);
  }

  .value.null {
    color: var(--null-color);
  }

  .value.date {
    color: var(--date-color);
  }

  p {
    color: var(--empty-text-color);
    margin: 0;
  }
</style>
