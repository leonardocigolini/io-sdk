<script>
  export let icon = "fas fa-home";
  export let description = "Home";
  export let to = "/";
  export let segment;
</script>

<li>
  <a href={to} class="large list-item" rel="noopener noreferrer" aria-current="{segment === undefined ? 'page' : undefined}">
      <i class={icon} />
      {description}
  </a>
</li>
