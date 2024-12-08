<script>
    import { petDatabase } from "./petDatabase.js";

    export let filters = {
        breed: "",
        species: "",
        location: "",
        age: "",
        specialNeeds: false,
    };

    const uniqueOptions = (key) => [
        ...new Set(petDatabase.map((pet) => pet[key])),
    ];

    const breeds = uniqueOptions("breed");
    const species = uniqueOptions("species");
    const locations = uniqueOptions("location");
    const ages = ["Baby", "Young", "Adult", "Old"];

    // Emit filter changes in real-time
    const updateFilters = () => {
        const event = new CustomEvent("filterChange", {
            detail: { ...filters },
        });
        dispatchEvent(event);
        console.log('hit');
        console.log(uniqueOptions);
    };
</script>

<div class="filter-pane">
    <h3 class="filter-title">Filters</h3>

    <div class="filter-item">
        <label for="breed">Breed</label>
        <select id="breed" bind:value={filters.breed} on:change={updateFilters}>
            <option value="">Select breed</option>
            {#each breeds as breed}
                <option value={breed}>{breed}</option>
            {/each}
        </select>
    </div>

    <div class="filter-item">
        <label for="species">Species</label>
        <select
            id="species"
            bind:value={filters.species}
            on:change={updateFilters}
        >
            <option value="">Select species</option>
            {#each species as sp}
                <option value={sp}>{sp}</option>
            {/each}
        </select>
    </div>

    <div class="filter-item">
        <label for="location">Location</label>
        <select
            id="location"
            bind:value={filters.location}
            on:change={updateFilters}
        >
            <option value="">Select location</option>
            {#each locations as location}
                <option value={location}>{location}</option>
            {/each}
        </select>
    </div>

    <div class="filter-item">
        <label for="age">Age</label>
        <select id="age" bind:value={filters.age} on:change={updateFilters}>
            <option value="">Select age</option>
            {#each ages as age}
                <option value={age}>{age}</option>
            {/each}
        </select>
    </div>

    <div class="filter-item">
        <label>
            <input
                type="checkbox"
                bind:checked={filters.specialNeeds}
                on:change={updateFilters}
            />
            Special Needs
        </label>
    </div>

</div>

<style>
    .filter-pane {
        position: fixed;
        top: 150px;
        left: 0;
        bottom: 0;
        padding: 1rem;
        border-style: dashed;
        color: rgb(22, 140, 140);
    }

    .filter-pane h3 {
        margin-top: 0px;
        margin-bottom: 30px;
        font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
        font-size: 26px;
    }

    .filter-item {
        margin-bottom: 1rem;
        font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
    }

    label {
        display: block;
        font-size: 1.2rem;
        text-align: left;
    }

    select,
    input[type="text"] {
        width: 100%;
        padding: 0.5rem;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    button {
        padding: 0.5rem 1rem;
        background-color: rgb(22, 140, 140);

        color: white;
        border: none;
        border-radius: 4px;
    }

    button:hover {
        background-color: rgb(14, 88, 88);
    }
</style>
