<script context="module">
  export async function preload({ params, query}) {
    let action = query.action;
    return {action}
  }
</script>

<script>
  export let action;
 // export let url;

  import * as api from 'api';
  import { formData } from "../../components/store";
  import { onMount } from "svelte";
  import ImportForm from "./ImportForm.svelte";
  import ImportData from "./ImportData.svelte";
  import ImportPreview from "./ImportPreview.svelte";

  let loading = true;
  let state = {};
  let message = "uploading...";
  let isPreview = false;

  async function loadData() {
    
    console.log(state);
    state  = await api.msg.load(action);
    loading = false;
  }

  function confirm() {
    console.log('confirm');
    loading = true;
    isPreview = false;
		saveData(state.data);
	}

  async function saveData(data) {
    res = await api.msg.save(data);
    if (res.ok) {
      message = "OK";
    } else {
      message = res.statusText
    }
    loading = false;
  }

  onMount(loadData);

  formData.subscribe(value => {
    state = value;
    if ("data" in state && !isPreview) 
       saveData(state.data);
  });
</script>

<h2>Import</h2>
<div>
  {#if loading}
    <div>loading...</div>
  {:else if state.form}
    <ImportForm form={state.form} {action} />
    <input type=checkbox bind:checked={isPreview}> Mostra anteprima
  {:else if state.error}
    <div class="alert alert-danger" role="alert">Error: {state.error}</div>
    <div>
      <button type="button" class="btn btn-primary" on:click={loadData}>
        Retry
      </button>
    </div>
  {:else if state.data}
    {#if !isPreview}
      <ImportData data={state.data} />
      <big><b>Import status: </b>{message}</big>
    {:else}
      <ImportPreview data={state.data} />
      {#if !loading}
        <button type="button" class="btn btn-primary" on:click={confirm}>
          Conferma importazione
        </button>
      {:else}
        <big><b>Import status: </b>{message}</big>
      {/if}
    {/if}
  {:else}
    <p>Cannot contact importer, please redeploy it.</p>
    <button type="button" class="btn btn-primary" on:click={loadData}>
      Retry
    </button>
  {/if}
</div>
