<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light main-nav">
    <a class="navbar-brand ms-3 main-nav" href="/">Biblioteka Publiczna</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active"><a class="nav-link" href="/books">Książki</a></li>
        <li class="nav-item active"><a class="nav-link" href="/authors">Autorzy</a></li>
        <li class="nav-item active"><a class="nav-link" href="/readers">Czytelnicy</a></li>
        <li class="nav-item active"><a class="nav-link" href="/rentals">Wypożyczenia</a></li>
      </ul>
    </div>
  </nav>
</template>

<script>
    export default {
      name: "CustomNavbar"
    }
</script>

<style scoped>
  .main-nav {
    font-size: 25px !important;
    color: #cadef3 !important;
    background-color: #2d373a !important;
  }

  .nav-link {
    color: #eaeaec !important;
    font-weight: 100;
    margin-right: 10px;
  }

  .login {
    background-color:transparent;
    border-color:transparent;
    color: gray;
  }

  body {
    size: 10px;
  }
</style>