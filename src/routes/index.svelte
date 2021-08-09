<script>
  let name = 'World'
  let action = 'Hello'
</script>

<h1>
  {action}
  {name}!
</h1>
<form class="row row-cols-lg-auto g-3 align-items-center">
  <div class="col">
    <input class="form-control" bind:value={name} />
  </div>
</form>
