<template>
  <div class="person">
    <div class="person__photo">
      <img :src="person.picture" alt="photo" />
    </div>
    <h2 class="person__name">{{ person.name }} ({{ person.age }})</h2>
    <a
      class="person__email"
      :href="`mailto:${person.email}`"
      aria-label="Электронная почта"
      target="_blank"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        height="24px"
        viewBox="0 0 24 24"
        width="24px"
        fill="currentColor"
      >
        <path d="M0 0h24v24H0V0z" fill="none" />
        <path
          d="M22 6c0-1.1-.9-2-2-2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6zm-2 0l-8 5-8-5h16zm0 12H4V8l8 5 8-5v10z"
        /></svg
    ></a>
    <div class="person__registry">
      <strong>Дата регистрации:</strong>
      {{ formatedDate }}
    </div>
    <p class="person__about"><strong>О себе:</strong> {{ person.about }}</p>
  </div>
</template>

<script>
export default {
  name: 'PersonCard',
  props: {
    person: {
      type: Object,
      default: null,
    },
  },
  computed: {
    formatedDate() {
      const options = {
        year: 'numeric',
        month: 'short',
        day: 'numeric',
        weekday: 'short',
      };
      const date = new Date(this.person.registered);
      return new Intl.DateTimeFormat('ru-RU', options).format(date);
    },
  },
};
</script>

<style scoped>
.person {
  display: grid;
  grid-template-columns: 50px 1fr;
  grid-gap: 10px;

  grid-template-rows: min-content min-content 1fr;
}

.person__photo img {
  height: 50px;
  width: 50px;
  border-radius: 50%;
}

.person__about {
  margin: 0;
  grid-column: 1 / -1;
}

.person__email {
  display: flex;
  justify-content: center;
  align-items: center;
  align-self: start;
  justify-self: center;
  padding: 0.4rem;
  color: #fff;
  background: #255af6;
  border-radius: 8px;
  transition: opacity 0.3s;
}

.person__email:hover {
  opacity: 0.8;
}

.person__name {
  margin: 0;
  align-self: center;
  font-size: 22px;
  color: #255af6;
}

.person__registry {
  display: flex;
  flex-direction: column;
}
</style>
