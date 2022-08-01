<script>
  let month = new Date().getMonth() + 1;
  $: d = new Date(2022, month - 1);

  function getDay(date) {
    // get day number from 0 (monday) to 6 (sunday)
    let day = date.getDay();
    if (day == 0) day = 7; // make Sunday (0) the last day
    return day - 1;
  }

  function toggleSelected(e) {
    e.target.classList.toggle("selected");
  }
</script>

<main>
  <p class="warn">
    <b>For testing only!</b>
    <span>Double-click or press space (when active) on selected date to select.</span>
    <label>
      Change month:
      <input type="number" bind:value={month} />
    </label>
  </p>
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
                tabindex="0"
                class={`day normal ${
                  7 * y + i + 1 - getDay(d) ===
                    new Date().getDay() &&
                  d.toLocaleDateString("en") ==
                    new Date().toLocaleDateString("en") &&
                  "current"
                }`}
                on:dblclick={toggleSelected}
                on:keypress={(e) => e.key === ' ' && toggleSelected(e)}
              >
                {7 * y + i + 1 - getDay(d)}
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
    padding: 1rem 1.5rem;
    cursor: pointer;
    min-width: 56px;
    border: 2px solid transparent;

    user-select: none;
    font-size: 1.8rem;
    margin: 0.5rem;
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

  td.selected {
    background-color: #3e72ba;
  }

  td.normal.selected {
    background-color: #3c67a450;
  }

  .normal:focus,
  .normal:active {
    outline: none;
    border: 2px solid #466ca1;
    background: #434954;
  }

  /** Dev only */
  .warn {
    text-align: center;
    background-color: #cd0f0f;
    border-radius: 3px;
    padding: 0.5rem;
  }

  .warn label {
    display: block;
  }
</style>
