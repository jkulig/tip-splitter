<script>
  import Splitter from "./lib/Splitter.svelte";

  import Logo from "./assets/logo.svg";
  
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true">
  <link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@700&display=swap" rel="stylesheet">
</svelte:head>

<header>
  <img src={Logo} alt="Splitter" />
</header>

<main>
 <Splitter />
</main>
