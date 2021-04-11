<script>
  export let name;
  const url = "https://localhost:44393/api/personas";
  let ListPersonas = [];
  let Persona = {
    nombre: "",
    apellido: "",
    edad: "",
  };
  import axios from "axios";
  let editarPerosnaEstado = false;
  window.onload = getUser();

  async function getUser() {
    try {
      const response = await axios.get(url);
      console.log(response.data);
      var p = response.data;
      ListPersonas = p;
    } catch (error) {
      console.error(error);
    }
  }
  const agregarPersona = (e) => {
    e.preventDefault();
    const newPersona = {
      nombre: Persona.nombre,
      apellido: Persona.apellido,
      edad: Persona.edad,
    };
    console.log("entro");
    const response = axios.post(url, newPersona);
    clearPersona();
  };
  const clearPersona = () => {
    Persona = {
      id: "",
      nombre: "",
      apellido: "",
      edad: "",
    };
  };

  const EscuhaPersona = (e) => {
    e.preventDefault();
    if (!editarPerosnaEstado) {
      agregarPersona(e);
    } else {
      EditarPersona(e);
    }
  };
  const deletePerosna = (id) => {
    let u = url + "/" + id;
    axios.delete(u);
    getUser();
    clearPersona();
  };
  const EditarPersona = (e) => {
 
    let u = url + "/" + 2;
    console.log(Persona.id);
    axios({
      method: "Put",

      url: u,
      data: {
        Nombre: Persona.nombre,
        Apellido: Persona.apellido,
        Edad: Persona.edad,
        Id: Persona.id,
      },
    });
    clearPersona();
    editarPerosnaEstado = false;
  };
  const editarPerosna = (personaEditable) => {
    Persona = personaEditable;
    editarPerosnaEstado = true;
  };
</script>

<main>
  <div class="container">
    <div class="row">
      <h1>Hello {name}!</h1>
      <div class="col-4 p-3">
        <form on:submit={EscuhaPersona}>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="Nombre">Nombre</span>
            </div>
            <input
              type="text"
              class="form-control"
              placeholder="Nombre"
              aria-label="Nombre"
              aria-describedby="Nombre"
              bind:value={Persona.nombre}
            />
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="Apellidos">Apellidos</span>
            </div>
            <input
              type="text"
              class="form-control"
              placeholder="Apellidos"
              aria-label="Apellidos"
              aria-describedby="Apellidos"
              bind:value={Persona.apellido}
            />
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="Edad">Edad</span>
            </div>
            <input
              type="number"
              class="form-control"
              placeholder="Edad"
              aria-label="Edad"
              aria-describedby="Edad"
              bind:value={Persona.edad}
            />
          </div>
          {#if !editarPerosnaEstado}
            <button class="btn btn-primary ">agregar</button>
          {:else}
            <button class="btn btn-primary ">Editar</button>
          {/if}
        </form>
      </div>
      {#if ListPersonas.length > 0}
        <div class="col-8">
          <table class="table table-striped  table-bordered">
            <thead>
              <tr>
                <th scope="col">#id</th>
                <th scope="col">Nombre</th>
                <th scope="col">Apellido</th>
                <th scope="col">Edad</th>
                <th />
              </tr>
            </thead>
            <tbody>
              {#each ListPersonas as item}
                <tr>
                  <th scope="row">{item.id}</th>
                  <td>{item.nombre}</td>
                  <td>{item.apellido}</td>
                  <td>{item.edad}</td>
                  <td>
                    <button
                      type="button"
                      class="btn btn-danger"
                      on:click={deletePerosna(item.id)}>Eliminar</button
                    >
                    <button
                      type="button"
                      class="btn btn-warning"
                      on:click={editarPerosna(item)}
                    >
                      Editar</button
                    >
                  </td>
                </tr>
              {/each}
            </tbody>
          </table>
        </div>
      {/if}
    </div>
  </div>
</main>

<style>
</style>
