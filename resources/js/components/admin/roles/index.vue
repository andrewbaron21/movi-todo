<template>
	<section>
		<div class="my-4 text-right">
			<button class="btn btn-primary" @click="addUser">Agregar Usuario</button>
		</div>
		<div class="table-responsive">
			<table id="table" class="table table-bordered table-hover table-wrap table-sm">
				<thead class="text-center">
					<tr>
						<th>Name</th>
						<th>Email</th>
						<th>Numero de cedula</th>
						<th>Role</th>
						<th>Actions</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="(user, index) in users" :key="index">
						<td>{{ user.name }}</td>
						<td>{{ user.email }}</td>
						<td>{{ user.identification_card }}</td>
						<td>{{ user.role_user.join(' - ')}} </td>
						<td>
							<div class="actions">
								<a href='#' role='edit'><i role='edit' @click.prevent="edit(user)">Edit</i></a>
								<button class="btn btn-danger btn-sm" @click="deleteUser(user, index)">Eliminar</button>
							</div>

						</td>
					</tr>
				</tbody>
			</table>
		</div>
		<form-user :user="user" :roles="roles" :type="type" />
	</section>
</template>

<script>
	import Form from "./Form";
	import datatables from "datatables";

	export default {
		props: ["users", "roles"],
		components: {
			"form-user": Form,
		},
		data() {
			return {
				user: {},
				type: "created",
			};
		},
		mounted() {
			this.mytable();
		},
		methods: {
			mytable() {
				$(document).ready(function () {
					$("#table").DataTable();
				});
			},
			addUser() {
				this.type = "created";
				$("#modalEditUser").modal({
					backdrop: "static",
					keyboard: false,
				});
			},
			edit(user) {
				this.user = user;
				this.type = "updated";
				$("#modalEditUser").modal({
					backdrop: "static",
					keyboard: false,
				});
			},
			async deleteUser(user, index) {
				var url = "";
				url = `/moviTodo/delete/${user.id}`;

				await axios.post(url, user).then((res) => {
					location.reload();
				});
			},
		},
	};
</script>
