<script>
  import { browser } from "$app/environment";
  import { onDestroy, onMount } from "svelte";

  let map;
  let mapElement;

  onMount(async () => {
    const L = await import("leaflet");
    await import("@geoman-io/leaflet-geoman-free");

    if (browser) {
      map = L.map(mapElement).setView([51.505, -0.09], 13);

      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution:
          '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      }).addTo(map);

      // Bind the marker
      L.marker([51.5, -0.09])
        .addTo(map)
        .bindPopup("A pretty CSS3 popup.<br> Easily customizable.")
        .openPopup();

      // add Leaflet-Geoman controls
      map.pm.addControls({
        position: "topleft",
      });
    }
  });

  onDestroy(async () => {
    if (map) {
      map.remove();
    }
  });
</script>

<svelte:head>
  <title>Geoman</title>
  <meta name="description" content="Geoman setup" />
</svelte:head>

<main>
  <div bind:this={mapElement} />
</main>

<style>
  @import "leaflet/dist/leaflet.css";
  @import "@geoman-io/leaflet-geoman-free/dist/leaflet-geoman.css";

  main div {
    height: 800px;
  }
</style>
