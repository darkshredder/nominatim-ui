<script>
  import DetailsLink from '../components/DetailsLink.svelte';
  import PageLink from '../components/PageLink.svelte';
  import {
    formatPlaceType, osmLink, formatAdminLevel, formatDistance
  } from '../lib/helpers.js';

  export let addressLine;
  export let bDistanceInMeters;
  export let bMarkUnusedLines = false;

  $: bAddressLineUsed = addressLine.isaddress;

</script>

<tr class:notused={bMarkUnusedLines && !bAddressLineUsed}>
  <td class="name font-weight-bold">
    {#if addressLine.localname}
      {addressLine.localname}
    {:else}
      <span class="noname">No Name</span>
    {/if}
  </td>
  <td>{formatPlaceType(addressLine)}</td>
  <td>{@html osmLink(addressLine)}</td>
  <td>{addressLine.rank_address}</td>
  <td>{formatAdminLevel(addressLine.admin_level)}</td>
  <td>{@html formatDistance(addressLine.distance, bDistanceInMeters)}</td>
  <td>
    {#if addressLine.osm_id}
      <DetailsLink feature={addressLine}>details</DetailsLink>
    {:else if addressLine.type.match(/^country/)}
      <PageLink page='search' params_hash={{ country: addressLine.localname }}>search by name</PageLink>
    {:else if addressLine.type === 'postcode'}
      <PageLink page='search' params_hash={{ postalcode: addressLine.localname }}>search by name</PageLink>
    {/if}
  </td>
</tr>

<style>
  .notused {
    color:#ddd;
  }

  td {
    padding: 2px 8px;
    font-size: 0.9em;
  }
</style>
