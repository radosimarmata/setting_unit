<script>
  import SelectWithSearch from "./components/SelectWithSearch.svelte";
  let primeMovers = [
    { id: 1, name: "PM001", type: 1, status: true },
    { id: 2, name: "PM002", type: 1, status: true },
    { id: 3, name: "PM003", type: 1, status: true },
    { id: 4, name: "PM004", type: 1, status: true },
    { id: 5, name: "PM005", type: 1, status: true },
    { id: 6, name: "PM006", type: 1, status: true },
    { id: 7, name: "PM007", type: 1, status: true },
    { id: 8, name: "PM008", type: 1, status: true },
    { id: 9, name: "PM009", type: 1, status: true },
    { id: 10, name: "PM010", type: 1, status: true },
  ];
  let vessels = [
    { id: 11, name: "V001", type: 2, status: false },
    { id: 12, name: "V002", type: 2, status: false },
    { id: 13, name: "V003", type: 2, status: false },
    { id: 14, name: "V004", type: 2, status: false },
    { id: 15, name: "V005", type: 2, status: false },
    { id: 16, name: "V006", type: 2, status: false },
    { id: 17, name: "V007", type: 2, status: false },
    { id: 18, name: "V008", type: 2, status: false },
    { id: 19, name: "V009", type: 2, status: false },
    { id: 20, name: "V010", type: 2, status: false },
  ];

  let dollies = [
    { id: 21, name: "D001", type: 3, status: false },
    { id: 22, name: "D002", type: 3, status: false },
    { id: 23, name: "D003", type: 3, status: false },
    { id: 24, name: "D004", type: 3, status: false },
    { id: 25, name: "D005", type: 3, status: false },
    { id: 26, name: "D006", type: 3, status: false },
    { id: 27, name: "D007", type: 3, status: false },
    { id: 28, name: "D008", type: 3, status: false },
    { id: 29, name: "D009", type: 3, status: false },
    { id: 30, name: "D010", type: 3, status: false },
  ];

  let tableData = primeMovers.map((primeMover) => ({
    primeMover,
    frontVessel: null,
    dolly: null,
    rearVessel: null,
    frontVesselEdit: false,
    rearVesselEdit: false,
    dollyEdit: false
  }));
  
  function removeItemFromList(item) {
    vessels = vessels.filter((vessel) => vessel !== item);
    dollies = dollies.filter((dolly) => dolly !== item);
  }


  function toggleEdit(rowIndex, type) {
    if( type === "front"){
      tableData[rowIndex].frontVesselEdit = !tableData[rowIndex].frontVesselEdit;
    } else if (type === "dolly"){
      tableData[rowIndex].dollyEdit = !tableData[rowIndex].dollyEdit;
    } else {
      tableData[rowIndex].rearVesselEdit = !tableData[rowIndex].rearVesselEdit;
    }
  }

  function handleFrontVesselChange(rowIndex, e) {
    const selectedVesselId = e;
    const selectedVessel = vessels.find((vessel) => vessel.id == selectedVesselId.id);
    const target = tableData[rowIndex];

    if (target.frontVessel) {
      vessels.push(target.frontVessel);
    }

    if (selectedVessel) {
      target.frontVessel = selectedVessel;
      removeItemFromList(selectedVessel);
      toggleEdit(rowIndex, "front");
    } else {
      target.frontVessel = null;
    }
  }

  function handleDollyChange(rowIndex, e) {
    const selectedDollyId = e;
    const selectedDolly = dollies.find((d) => d.id == selectedDollyId.id);
    const target = tableData[rowIndex];

    if (target.dolly) {
      dollies.push(target.dolly);
    }

    if (selectedDolly) {
      target.dolly = selectedDolly;
      removeItemFromList(selectedDolly);
      toggleEdit(rowIndex, "dolly");
    } else {
      target.frontVessel = null;
    }
  }

  function handleRearVesselChange(rowIndex, e) {
    const selectedVesselId = e;
    const selectedVessel = vessels.find((vessel) => vessel.id == selectedVesselId.id);
    const target = tableData[rowIndex];

    if (target.rearVessel) {
      vessels.push(target.rearVessel);
    }

    if (selectedVessel) {
      target.rearVessel = selectedVessel;
      removeItemFromList(selectedVessel);
      toggleEdit(rowIndex, "rear");
    } else {
      target.frontVessel = null;
    }
  }

  function handlePaperPlaneClick(row) {
    // Jika belum memilih vessel, dolly, atau rearVessel
    if (!row.frontVessel && !row.dolly && !row.rearVessel) {
      return console.log("Harap pilih Vessel");
    }

    // Jika ada dolly, pastikan frontVessel dan rearVessel ada
    if (row.dolly && (!row.frontVessel || !row.rearVessel)) {
      return console.log("Harap isi front vessel dan rear vessel");
    }

    // Jika semua syarat terpenuhi
    console.log("Row yang dipilih: ", row);
  }

  function handleDeleteRow(index) {
    tableData.splice(index, 1);
  }

  let selectedOption = '';  // Nilai yang dipilih
  let options = [
    "Dolly 1",
    "Dolly 2",
    "Dolly 3",
    "Dolly 4",
    "Dolly 5"
  ];

  function handleSelectChange(option) {
    selectedOption = option;
    console.log("Selected Option:", selectedOption);
  }

</script>

<main>
  <div class="container-full">
    <h3 class="text-center py-3">Setting Unit</h3>
    <div class="main-content">
      <div class="col-md-3 standby-column">
        <div>
          <h5>Vessel Standby</h5>
          <div class="standby-list">
            {#each vessels as vessel}
              <div
                class="standby-item"
              >
                {vessel.name}
              </div>
            {/each}
          </div>
        </div>
        <div>
          <h5>Dolly Standby</h5>
          <div class="standby-list">
            {#each dollies as dolly}
              <div
                class="standby-item"
              >
                {dolly.name}
              </div>
            {/each}
          </div>
        </div>
      </div>

      <div class="col-md-9 table-container">
        <table class="table table-striped table-hover table-sm">
          <thead>
            <tr>
              <th>Prime Mover</th>
              <th>Front Vessel</th>
              <th>Dolly</th>
              <th>Rear Vessel</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            {#each tableData as row, index}
              <tr>
                <td>{row.primeMover.name}</td>
                <td>
                  {#if row.frontVesselEdit}
                    <!-- <select
                      class="form-select form-select-sm"
                      on:change={(event) => handleFrontVesselChange(index, event)}
                      value={row.frontVessel?.id || ""}
                    >
                      <option value="">Select Vessel</option>
                      {#each vessels.filter(vessel => !tableData.some(r => r.frontVessel?.id === vessel.id && r !== row)) as vessel}
                        <option value={vessel.id}>
                          {vessel.name}
                        </option>
                      {/each}
                    </select> -->
                    <SelectWithSearch 
                      options={vessels.filter(vessel => !tableData.some(r => r.frontVessel?.id === vessel.id && r !== row))} 
                      value={row.frontVessel} 
                      onChange={(event) => handleFrontVesselChange(index, event)}
                    />
                  {:else}
                    <button class="btn btn-outline-primary btn-sm" on:click={() => toggleEdit(index, "front")}>{row.frontVessel ? row.frontVessel.name : "Vessel"}</button>
                  {/if}
                </td>
                <td>
                  {#if row.dollyEdit}
                    <SelectWithSearch 
                      options={dollies.filter(d => !tableData.some(r => r.dolly?.id === d.id && r !== row))} 
                      value={row.dolly} 
                      onChange={(event) => handleDollyChange(index, event)}
                    />
                  {:else}
                    <button class="btn btn-outline-primary btn-sm" on:click={() => toggleEdit(index, "dolly")}>{row.dolly ? row.dolly.name : "Dolly"}</button>
                  {/if}
                </td>
                <td>
                  {#if row.rearVesselEdit}
                    <SelectWithSearch 
                      options={vessels.filter(vessel => !tableData.some(r => r.rearVessel?.id === vessel.id && r !== row))} 
                      value={row.rearVessel} 
                      onChange={(event) => handleRearVesselChange(index, event)}
                    />
                  {:else}
                    <button class="btn btn-outline-primary btn-sm" on:click={() => toggleEdit(index, "rear")}>{row.rearVessel ? row.rearVessel.name : "Vessel"}</button>
                  {/if}
                </td>
                <td>
                  <button class="btn btn-danger" on:click={() => handleDeleteRow(index)}>
                    <i class="fa fa-eraser"></i>
                  </button>
                  <button class="btn btn-success" on:click={() => handlePaperPlaneClick(row)}>
                    <i class="fa fa-paper-plane"></i>
                  </button>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </div>
  </div>
</main>