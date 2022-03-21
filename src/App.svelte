<script>
	let empleados = [];
	let activado = true;
	let datosEmpleado = {
		id: null,
		nombre: "",
		correo: "",
	};
	let mostrarEmpleados = () => {
		fetch("https://www.eduenvio.com/")
			.then((respuesta) => respuesta.json())
			.then((datosRespuesta) => {
				empleados = datosRespuesta;
				datosEmpleado = {
					id: null,
					nombre: "",
					correo: "",
				};
				activado = true;
				console.log(empleados);
			})
			.catch(console.log);
	};

	let agregarEmpleado = () => {
		const nuevoEmpleado = {
			id: datosEmpleado.id,
			nombre: datosEmpleado.nombre,
			correo: datosEmpleado.correo,
		};

		fetch("https://www.eduenvio.com/?insertar=1", {
			method: "POST",
			body: JSON.stringify(nuevoEmpleado),
		})
			.then((respuesta) => respuesta.json())
			.then((datosRespuesta) => {
				console.log(datosRespuesta);
				mostrarEmpleados();
			})
			.catch(console.log);

		console.log(nuevoEmpleado);
	};

	let borrarEmpleado = (id) => {
		fetch("https://www.eduenvio.com/?borrar=" + id)
			.then((respuesta) => respuesta.json())
			.then((datosRespuesta) => {
				mostrarEmpleados();
			})
			.catch(console.log);
	};

	let editarEmpleado = (empleado) => {
		activado = false;
		datosEmpleado = empleado;
	};

	let actualizarEmpleado = () => {
		fetch("https://www.eduenvio.com/?actualizar=" + datosEmpleado.id, {
			method: "POST",
			body: JSON.stringify(datosEmpleado),
		})
			.then((respuesta) => respuesta.json())
			.then((datosRespuesta) => {
				console.log(datosRespuesta);
				mostrarEmpleados();
			})
			.catch(console.log);
	};

	mostrarEmpleados();
</script>

<main>
	<div class="container">
		<div class="row">
			<div class="col-sm-5">
				<div class="card">
					<div class="card-header">Empleados</div>
					<div class="card-body">
						<form action="">
							<div class="mb-3">
								<label for="" class="form-label"> ID </label>
								<input
									readonly
									bind:value={datosEmpleado.id}
									type="text"
									class="form-control"
									name=""
									id=""
									aria-describedby="helpId"
									placeholder=""
								/>
							</div>
							<div class="mb-3">
								<label for="" class="form-label">
									Nombre
								</label>
								<input
									bind:value={datosEmpleado.nombre}
									type="text"
									class="form-control"
									name=""
									id=""
									aria-describedby="helpId"
									placeholder=""
								/>
							</div>

							<div class="mb-3">
								<label for="" class="form-label">
									Correo
								</label>
								<input
									bind:value={datosEmpleado.correo}
									type="text"
									class="form-control"
									name=""
									id=""
									aria-describedby="helpId"
									placeholder=""
								/>
							</div>

							<button
								type="button"
								class="btn btn-primary"
								on:click|preventDefault={agregarEmpleado}
								disabled={!activado}>Agregar Empleado</button
							>
							<button
								type="button"
								class="btn btn-primary"
								on:click|preventDefault={actualizarEmpleado}
								disabled={activado}
								>Actualizar
							</button>

							<button
								type="button"
								class="btn btn-danger"
								on:click|preventDefault={mostrarEmpleados}
								>Cancelar
							</button>
						</form>
					</div>
				</div>
			</div>
			<div class="col-sm-7">
				<table class="table">
					<thead>
						<tr>
							<th>ID</th>
							<th>Nombre</th>
							<th>Correo</th>
							<th>Acciones</th>
						</tr>
					</thead>
					<tbody>
						{#each empleados as empleado}
							<tr>
								<td>{empleado.id}</td>
								<td>{empleado.nombre}</td>
								<td>{empleado.correo}</td>
								<td
									><button
										type="submit"
										class="btn btn-primary"
										on:click={editarEmpleado(empleado)}
										>Editar</button
									>

									|
									<button
										type="submit"
										class="btn btn-danger"
										on:click={borrarEmpleado(empleado.id)}
										>Borrar</button
									></td
								>
							</tr>
						{/each}
					</tbody>
				</table>
			</div>
		</div>
	</div>
</main>

<style>
</style>
