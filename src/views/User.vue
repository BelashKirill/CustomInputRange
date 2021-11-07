<template>
    <div>Страница пользователя с ID = {{ $route.params.id }}</div>
    <router-link :to="'/'">Назад</router-link>
</template>

<script>
export default {
  name: 'User'
}
</script>