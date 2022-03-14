<template name="users">
  <div class="container pt-3">
    <h1>{{ title }}</h1>

    <form class="mb-3" @submit.prevent="addContact">
      <div class="form-group mb-3">
        <label for="name" class="mr-3">Ім'я</label>
        <input
          type="text"
          class="form-control"
          id="name"
          v-model.trim="form.first_name"
        />
      </div>
      <div class="form-group mb-3">
        <label for="last_name" class="mr-3">Прізвище</label>
        <input
          type="text"
          class="form-control"
          id="last_name"
          v-model="form.last_name"
        />
      </div>
      <div class="form-group mb-3">
        <label for="phone" class="mr-3">Телефон</label>
        <input
          type="text"
          class="form-control"
          id="phone"
          v-model="form.phone"
          placeholder="+38(050)123-45-89"
        />
        <span class="plus" @click="addFilds1">+</span>
      </div>
      <div class="form-group mb-3" v-if="showFilds1">
        <label for="phone1" class="mr-3">Телефон</label>
        <input
          type="text"
          class="form-control"
          id="phone1"
          v-model="form.phone1"
          placeholder="+38(050)123-45-89"
        />
        <span class="plus" @click="addFilds2">+</span>
      </div>
      <div class="form-group mb-3" v-if="showFilds2">
        <label for="phone2" class="mr-3">Телефон</label>
        <input
          type="text"
          class="form-control"
          id="phone2"
          v-model="form.phone2"
          placeholder="+38(050)123-45-89"
        />
      </div>
      <button class="btn btn-primary mt-3" type="submit">Додати</button>
    </form>

    <div v-if="contacts.length">
      <div class="card mb-3" v-for="(contact, index) in contacts" :key="index">
        <div class="card-block">
          <div class="card-body">
            <h5 class="card-title">
              {{ contact.first_name }}
            </h5>
            <h5 class="card-title">
              {{ contact.last_name }}
            </h5>
            <div class="card-number">
              <p class="card-text">{{ contact.phone }}</p>
              <p class="card-text">{{ contact.phone1 }}</p>
              <p class="card-text">{{ contact.phone2 }}</p>
            </div>
          </div>
          <div class="card-btn">
            <button class="btn btn-danger" @click="removeContact(index)">
              Видалити
            </button>
          </div>
        </div>
      </div>
    </div>
    <p v-else>Контактів нема</p>
  </div>
</template>

<script>
export default {
  name: "users",
  data: () => {
    return {
      title: "Список контактів",
      showFilds1: false,
      showFilds2: false,
      form: {
        first_name: null,
        last_name: null,
        phone: [],
      },
      contacts: [],
    };
  },
  methods: {
    addContact(e) {
      e.preventDefault();
      if (!this.form.first_name) {
        alert("Введіть ім'я!");
      } else if (!this.form.last_name) {
        alert("Введіть прізвище!");
      } else if (!this.form.phone.length) {
        alert("Введіть номер телефону!");
      } else {
        this.contacts.push(this.form);
        this.saveContacts();
        setTimeout(() => {
          this.form.first_name = "";
          this.form.last_name = "";
          this.form.phone = "";
        }, 500);
        location.reload();
      }
    },
    removeContact(x) {
      this.contacts.splice(x, 1);
      this.saveContacts();
    },
    saveContacts() {
      let parsed = JSON.stringify(this.contacts);
      localStorage.setItem("contacts", parsed);
    },
    addFilds1() {
      this.showFilds1 = true;
    },
    addFilds2() {
      this.showFilds2 = true;
    },
  },
  mounted() {
    if (localStorage.getItem("contacts")) {
      try {
        this.contacts = JSON.parse(localStorage.getItem("contacts"));
      } catch (e) {
        localStorage.removeItem("contacts");
      }
    }
  },
};
</script>

<style scoped lang="scss">
.form-group {
  max-width: 600px;
  width: 100%;
  position: relative;
  .plus {
    position: absolute;
    display: block;
    top: -8px;
    left: 75px;
    font-size: 25px;
    font-weight: 800;
    color: green;
    cursor: pointer;
  }
}
.card {
  max-width: 800px;
  width: 100%;
}
.card-block {
  display: flex;
  justify-content: space-between;
  align-items: center;
  .card-body {
    display: flex;
    flex-wrap: wrap;
    .card-title {
      margin-right: 15px;
    }
    .card-number {
      display: flex;
      .card-text {
        margin-right: 10px;
      }
    }
  }
  .card-btn {
    padding: 1rem 1rem;
  }
}
@media screen and (max-width: 770px) {
  .card-number {
    flex-direction: column;
  }
}
@media (min-width: 576px) {
  .form-inline .form-control {
    width: 100%;
  }
}
@media screen and (max-width: 490px) {
  .card-block {
    flex-direction: column;
  }
}
</style>
