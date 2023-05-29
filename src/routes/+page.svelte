<script lang="ts">
  import parties, { type Resultat } from '../notes'

  let partieActive = '--';
  let noteNum: number;
  let resultats: Resultat[] = []

  $: resultats = obtenirParties(partieActive, noteNum);

  function obtenirParties(partieActive: string, noteNum: number) {
    if (noteNum === undefined) return []
    if (partieActive === '--') {
      return parties.reduce((acc, partie) => {
        if (partie.notes[noteNum]) {
          return [
            ...acc,
            {
              nom: partie.nom,
              note: partie.notes[noteNum]
            }
          ]
        }
        return acc
      }, [] as Resultat[])
    }
    const noteTrouvee = parties.find(partie => partie.nom === partieActive)?.notes[noteNum]
    if (noteTrouvee) return [{
      nom: partieActive,
      note: noteTrouvee
    }]
    return []
  }

  function isUrl(str: string) {
    try {
      new URL(str);
      return true;
    } catch {
      return false;
    }
  }
</script>

<h2>Salomé Saqué</h2>
<div class="titre">
  <h1>Sois jeune<br>et tais-toi</h1>
</div>
<h3>Notes et références</h3>
<div>
  <select bind:value={partieActive}>
    <option value="--"> -- </option>
    {#each parties as partie}
      <option value={partie.nom}>{partie.nom}</option>
    {/each}
  </select>
</div>
<div>
  <input type="number" min="1" bind:value={noteNum} placeholder="#">
</div>

{#each resultats as resultat}
<div class="resultat">
  <h4>{resultat.nom}</h4>
  {#each resultat.note as note}
    {#if isUrl(note)}
      <p>
        <a href={note}>
          {note}
        </a>
      </p>
    {:else}
      <p>{note}</p>
    {/if}
  {/each}
</div>
{/each}

<style>
  input {
    font-size: 3rem;
    width: 30%;
    padding: 0.5rem;
    border: none;
    border: 1px solid black;
    text-align: center;
  }
  select {
    width: 50%;
    font-size: 1rem;
    padding: 0.5rem;
    border: none;
    border: 1px solid black;
    text-align: center;
    margin-bottom: 0.5rem;
  }
  h1 {
    font-family: 'Times New Roman', Times, serif;
    font-size: 4rem;
    color: brown;
    margin: 1rem;
  }
  h2 {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 2.3rem;
    font-weight: lighter;
    text-transform: uppercase;
    margin: 1rem;
  }
  .resultat {
    text-align: left;
  }
  .titre {
    background-color: white;
  }
</style>