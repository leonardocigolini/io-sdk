<script>
	import Topbar from '../components/Topbar.svelte';
	import Sidebar from '../components/Sidebar.svelte';
	import Cookiebar from '../components/Cookiebar.svelte';
	import Footer from '../components/Footer.svelte';

	export let segment;
</script>


<main>
  <div id="root">
    <Topbar/>
    <section class="row">
      <Sidebar {segment}/>
      <div class="col-9 col-md-offset-1 ">
        <div class="mr-1 pt-1 pb-1">
          <slot></slot>
        </div>
      </div>
    </section>
    <Cookiebar/>
    <Footer/>
  </div>
</main>
