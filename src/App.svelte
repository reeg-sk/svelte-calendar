<script>
  let month = new Date().getMonth() + 1;
  $: d = new Date(2022, month - 1);

  function getDay(date) {
    // get day number from 0 (monday) to 6 (sunday)
    let day = date.getDay();
    if (day == 0) day = 7; // make Sunday (0) the last day
    return day - 1;
  }
</script>

<main>
  <input type="number" bind:value={month} />
  <h1>
    {d.toLocaleDateString("en", { month: "long" })}
    {d.getFullYear()}
  </h1>
  <p>
    {new Date(
      d.getFullYear(),
      d.getMonth() + 1,
      0
    ).getDate()} days
  </p>

  <table>
    <thead>
      <tr>
        {#each ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"] as name}
          <th>
            {name}
          </th>
        {/each}
      </tr>
    </thead>
    <tbody>
      {#each { length: getDay(d) >= 5 && new Date(d.getFullYear(), d.getMonth() + 1, 0).getDate() >= 31 ? 6 : 5 } as _, y}
        <tr>
          {#each { length: 7 } as _, i}
            {#if (y === 0 && i < getDay(d)) || 7 * y + i + 1 - getDay(d) > new Date(d.getFullYear(), d.getMonth() + 1, 0).getDate()}
              <td class="day empty">
                <!-- empty -->
              </td>
            {:else}
              <td
                class={`day normal ${
                  7 * y + i + 1 - getDay(d) ===
                    new Date().getDay() &&
                  d.toLocaleDateString("en") ==
                    new Date().toLocaleDateString("en") &&
                  "current"
                }`}
              >
                <p>{7 * y + i + 1 - getDay(d)}</p>
              </td>
            {/if}
          {/each}
        </tr>
      {/each}
    </tbody>
  </table>
</main>

<style>
  h1 {
    margin-bottom: 0;
  }

  th {
    background: #4373ad;
    padding: 0.5rem;
    width: 4.2rem;
  }

  td {
    background: #484848;
    padding: 0.5rem 1rem;
    cursor: pointer;
    min-width: 56px;
  }

  td.empty {
    background: #3f3f3f;
    cursor: default;
  }

  td.normal:hover {
    background-color: #555555;
  }

  td.current {
    color: #54ffb8;
  }

  td p {
    font-size: 1.8rem;
    margin: 0.5rem;
  }
</style>
