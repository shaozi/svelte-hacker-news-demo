<script>
  import { browser } from '$app/env'
  import Toggle from 'svelte-toggle'

  let toggleTheme
  let darkTheme = false

  if (browser) {
    let theme = localStorage.getItem('theme')
    darkTheme = theme === 'dark'
    if (darkTheme) {
      document.body.classList.add('bootstrap-dark')
    }
    toggleTheme = () => {
      document.body.classList.toggle('bootstrap-dark')
      darkTheme = !darkTheme
      localStorage.setItem('theme', darkTheme ? 'dark' : 'light')
    }
  }
</script>

<nav class="navbar navbar-expand-sm navbar-themed">
  <div class="container">
    <ul class="navbar-nav">
      <li class="nav-item">
        <a class="nav-link" href="/">Home</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="/news">News</a>
      </li>
    </ul>
    <div class="navbar-nav">
      <Toggle label="" on="🌒" off="🌞" toggled={darkTheme} on:toggle={toggleTheme} />
    </div>
  </div>
</nav>

<div class="container">
  <slot />
</div>
