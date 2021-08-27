<template>
  <div id="app">
    <h1>controle de usuáriso</h1>
    <div>
      <h4>Listagem</h4>
      <ul>
        <li v-for="user in usuarios" :key="user._id">
          {{ user.firstName }} {{ user.lastName }}
          <button @click="getUserById(user._id)">Editar</button>
          <button @click="deleteUser(user._id)">Excluir</button>
        </li>
      </ul>
    </div>
    <div>
      <hr />
      <h4 v-if="action === 'add'">Cadastro</h4>
      <h4 v-else>Alterar</h4>
      <div>
        <label>Primeiro nome</label>
        <input type="text" v-model="firstName" />
      </div>
      <div>
        <label>Sobrenome</label>
        <input type="text" v-model="lastName" />
      </div>
      <div>
        <label>Idade</label>
        <input type="text" v-model="age" />
      </div>
      <div>
        <label>username</label>
        <input type="text" v-model="username" />
      </div>
      <div>
        <label>password</label>
        <input type="password" v-model="password" />
      </div>
      <button v-if="action === 'add'" @click="addUser()">Enviar</button>
      <button v-if="action === 'update'" @click="updateUser(userId)">
        Enviar
      </button>
      <p>{{ message }}</p>
    </div>
  </div>
</template>
<script>
export default {
  name: "App",
  data: function () {
    return {
      usuarios: [],
      firstName: "",
      lastName: "",
      age: "",
      username: "",
      password: "",
      message: "",
      action: "add",
      userId: "",
    };
  },
  methods: {
    getUser: async function () {
      const result = await fetch("http://localhost:3000")
        .then((res) => res.json())
        .then((res) => res)
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (!result.error) {
        this.usuarios = result;
      }
    },
    addUser: async function () {
      const newUser = {
        firstName: this.firstName,
        lastName: this.lastName,
        age: this.age,
        username: this.username,
        password: this.password,
      };
      if (newUser.firstName === "") {
        this.message = "Primeiro nome é obrigatório";
        return;
      }
      if (newUser.lastName === "") {
        this.message = "Sobrenome nome é obrigatório";
        return;
      }
      if (newUser.age === "") {
        this.message = "Idade nome é obrigatória";
        return;
      }
      if (newUser.username === "") {
        this.message = "Username nome é obrigatório";
        return;
      }
      if (newUser.password === "") {
        this.message = "Senha nome é obrigatório";
        return;
      }
      const result = await fetch("http://localhost:3000", {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(newUser),
      })
        .then((res) => res.json())
        .then((res) => res)
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (result.insertedId) {
        await this.getUser();
        this.message = "Usuário cadastrado com sucesso";
      }
    },
    deleteUser: async function (userId) {
      const result = await fetch("http://localhost:3000/" + userId, {
        method: "DELETE",
      })
        .then((res) => res.json())
        .then((res) => res)
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (result.deletedCount) {
        await this.getUser();
        this.message = "Usuário removido com sucesso";
      }
    },
    getUserById: function (userId) {
      const [usuario] = this.usuarios.filter((user) => user._id === userId);
      this.action = "update";
      this.userId = usuario._id;
      this.firstName = usuario.firstName;
      this.lastName = usuario.lastName;
      this.age = usuario.age;
      this.username = usuario.username;
      this.password = usuario.password;
    },
    updateUser: async function (userId) {
      const newUser = {
        firstName: this.firstName,
        lastName: this.lastName,
        age: this.age,
        username: this.username,
        password: this.password,
      };
      if (newUser.firstName === "") {
        this.message = "Primeiro nome é obrigatório";
        return;
      }
      if (newUser.lastName === "") {
        this.message = "Sobrenome nome é obrigatório";
        return;
      }
      if (newUser.age === "") {
        this.message = "Idade nome é obrigatória";
        return;
      }
      if (newUser.username === "") {
        this.message = "Username nome é obrigatório";
        return;
      }
      if (newUser.password === "") {
        this.message = "Senha nome é obrigatório";
        return;
      }
      const result = await fetch("http://localhost:3000/" + userId, {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "PUT",
        body: JSON.stringify(newUser),
      })
        .then((res) => res.json())
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (!result.error) {
        await this.getUser();
        this.message = "Usuário alterado com sucesso";
      }
    },
  },
  created: function () {
    this.getUser();
  },
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  padding: 30px;
}
label {
  display: block;
}
input {
  height: 30px;
  width: 300px;
  margin-bottom: 30px;
}
li {
  height: 50px;
  width: 400px;
  border-bottom: 1px solid #c1c1c1;
  padding: 10px;
  box-sizing: border-box;
}
li button {
  margin-left: 10px;
  float: right;
}
</style>