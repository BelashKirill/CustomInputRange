<template>
    <div>Страница пользователя с ID = {{ $route.params.id }}</div>
    <a href="/">Назад</a>
</template>

<script>
export default {
  name: 'User'
}
</script>