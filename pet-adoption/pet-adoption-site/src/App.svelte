<script>
  import Filters from "./lib/Filters.svelte";
  import SiteHeader from "./lib/SiteHeader.svelte";
  import { petDatabase } from "./lib/petDatabase.js";

  let filters = {
    breed: "",
    species: "",
    location: "",
    age: "",
    specialNeeds: false,
  };
  let filteredPets = petDatabase;
  let selectedPet = null; //To track the currently selected pet

  const viewPetDetails = (pet) => {
    selectedPet = pet;
  };
  const goBack = () => {
    selectedPet = null;
  };

  // Reactive declaration for filteredPets
  $: filteredPets = petDatabase.filter(
    (pet) =>
      (!filters.breed || pet.breed === filters.breed) &&
      (!filters.species || pet.species === filters.species) &&
      (!filters.location || pet.location === filters.location) &&
      (!filters.age || pet.age === filters.age) &&
      (!filters.specialNeeds || pet.specialNeeds),
  );

  const handleCardClick = (pet) => {
    alert(`Would you like to adopt ${pet.name}?`);
    // Replace the alert with navigation or modal logic to show detailed pet info
  };
</script>

<div class="layout">
  <SiteHeader></SiteHeader>
  <aside>
    <Filters bind:filters />
  </aside>
  <main>
    <grid>
      {#if selectedPet}
        <button on:click={goBack} class="back-button">Back</button>
        <div class="pet-view">
          <h2 class="pet-name">{selectedPet.name}</h2>
          <img
            src={selectedPet.image}
            alt={`Image of ${selectedPet.name}`}
            class="pet-image-fs"
          />
          <p><strong>Breed:</strong> {selectedPet.breed}</p>
          <p><strong>Species:</strong> {selectedPet.species}</p>
          <p><strong>Location:</strong> {selectedPet.location}</p>
          <p><strong>Age:</strong> {selectedPet.age}</p>
          {#if selectedPet.specialNeeds}
            <p><strong>Special Needs:</strong> Yes</p>
          {:else}
            <p><strong>Special Needs:</strong> No</p>
          {/if}
          <p>{selectedPet.description}</p>
        </div>
      {:else if filteredPets.length > 0}
        <div class="pet-grid">
          {#each filteredPets as pet}
            <div
              class="pet-card"
              on:click={() => viewPetDetails(pet)}
              role="button"
              tabindex="0"
            >
              <h4>{pet.name}</h4>
              <img
                src={pet.image}
                alt={`Image of ${pet.name}`}
                class="pet-image"
              />
              <div class="pet-details">
                <p><strong>Breed:</strong> {pet.breed}</p>
                <p><strong>Species:</strong> {pet.species}</p>
                <p><strong>Location:</strong> {pet.location}</p>
                <p><strong>Age:</strong> {pet.age}</p>
                {#if pet.specialNeeds}
                  <p>
                    <br />
                    <strong style={"color:red; font-size:24px;"}
                      >Special Needs</strong
                    >
                  </p>
                {/if}
              </div>
            </div>
          {/each}
        </div>
      {:else}
        <p>No pets found matching your criteria</p>
      {/if}
    </grid>
  </main>
</div>

<style>
  .layout {
    display: flex;
  }

  aside {
    width: 25%;
  }

  main {
    position: fixed;
    top: 150px;
    display: grid;
    padding: 1rem;
  }
  grid {
    position: fixed;
    top: 150px;
    right: 0px;
    left: 200px;
    bottom: 0px;
    color: rgb(22, 140, 140);
    overflow-y: scroll;
    border-style: dashed;
  }

  .pet-image {
    width: 100%;
    height: 150px;
    object-fit: cover;
    border-radius: 8px;
    margin-bottom: 0.5rem;
  }

  h4 {
    margin: 0.5rem 0;
    font-size: 1.2rem;
  }

  .pet-grid {
    padding-left: 10px;
    padding-top: 50px;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
  }

  .pet-card {
    border: 1px solid rgb(22, 140, 140);
    padding: 1rem;
    height: 400px;

    margin-bottom: 1rem;
    border-radius: 8px;
  }
  .pet-card:hover {
    transform: scale(1.02);
    box-shadow: 0 4px 8px rgb(22, 140, 140);
  }

  .pet-card:focus {
    outline: 2px solid rgb(22, 140, 140);
  }

  .pet-details {
    flex-direction: column;
  }

  h3 {
    color: rgb(22, 140, 140);
    font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
    font-size: 26px;
    margin-top: 0px;
    margin-bottom: 0px;
    position: fixed;
    right: 5px;
    left: 205px;
    height: 50px;
    text-align: center;
    background-color: azure;
  }

  p {
    margin: 0px;
  }

  .pet-view {
    padding: 2rem;
    text-align: center;
  }

  .pet-view .pet-image-fs {
    width: 50%;
    height: auto;
    object-fit: cover;
    border-radius: 8px;
    margin-bottom: 1rem;
  }

  .back-button {
    position: fixed;
    top:160px;
    left:210px;
    background-color:rgb(22, 140, 140);
    color: white;
    border: none;
    border-radius: 5px;
    padding: 0.5rem 1rem;
    margin-bottom: 1rem;
    cursor: pointer;
  
  }

  .back-button:hover {
    background-color: rgb(16, 109, 109);
  }
</style>
