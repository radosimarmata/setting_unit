<script>
  export let options = [];
  export let value = '';
  export let onChange = () => {};

  let searchQuery = '';
  let showDropdown = false;
  
  $: filteredOptions = options.filter(option => 
    option.name.toLowerCase().includes(searchQuery.toLowerCase())
  );

  function handleSearchChange(event) {
    searchQuery = event.target.value;
  }

  function handleSelect(option) {
    value = option;
    onChange(option);
    showDropdown = false;
  }

  function closeDropdown() {
    showDropdown = false;
  }

  function toggleDropdown() {
    showDropdown = true;
  }

  import { onMount } from 'svelte';
  onMount(() => {
    const clickOutside = (event) => {
      if (!event.target.closest('.dropdown-container')) {
        closeDropdown();
      }
    };
    document.addEventListener('click', clickOutside);
    return () => document.removeEventListener('click', clickOutside);
  });
</script>

<style>
  .no-results {
    padding: 8px;
    color: #999;
    z-index: 1000;
  }

  .dropdown-select {
    position: block;
    padding: 8px;
    cursor: pointer;
    background-color: white;
    border-bottom: 1px solid #eee;
    z-index: 999;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .form-control {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

</style>

<div class="dropdown-container">
  <input
    type="text"
    class="form-control"
    placeholder="Search..."
    bind:value={searchQuery}
    on:focus={toggleDropdown} 
    on:input={handleSearchChange}
  />

  {#if showDropdown} 
    {#if filteredOptions.length > 0}
      {#each filteredOptions as option}
        <div class="dropdown-select" on:click={() => handleSelect(option)}>
          {option.name}
        </div>
      {/each}
    {:else}
      <div class="no-results">No results found</div>  <!-- Menampilkan pesan jika tidak ada hasil -->
    {/if}
  {/if}
</div>
