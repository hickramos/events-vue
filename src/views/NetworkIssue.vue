<template>
  <div>
    <h1>Uh-Oh!</h1>
    <h3>
      It's looks like you're experiencing network issues, plese click to the
      back button and try again
    </h3>
    <router-link :to="{ name: 'event-list' }"
      >Or back to the home page</router-link
    >
  </div>
</template>
