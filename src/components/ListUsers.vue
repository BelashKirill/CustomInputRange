<template>
    <input class="form-control" list="datalistOptions" id="exampleDataList" placeholder="search..." v-model="search">
    <ul class="list-group list-group-flush group-flush">
        <li class="list-group-item" v-for="(name) in filteredList" :key="name.id">
            <span>{{name.username}}</span>
            <span>
                <router-link :to="'/users/' + name.id">Перейти</router-link>
                <span @click="deleteItem(name)">Удалить</span>
            </span>
        </li>
    </ul>
    <div class="wrap-add">
        <input class="form-control" list="datalistOptions" id="exampleDataList" placeholder="enter name..." v-model="name">
        <button type="button" class="btn btn-primary" @click="addItem">Добавить</button>
    </div>
</template>

<script>
export default {
    name: 'ListUsers',
    data() {
        return {
            search: '',
            users: [],
            name: ''
        }
    },
    computed: {
        filteredList() {
            return this.users.filter((item) => {
                return item.username.toLowerCase().includes(this.search.toLowerCase())
            })
        },
    },
    methods: {
        deleteItem(name) {
            this.users = this.users.filter((item) => item.id !== name.id)
        },
        addItem() {
            if (this.name) {
                if (!this.hasNewEl(this.name)) {
                    this.users.push({
                        id: this.getNewId(),
                        username: this.name
                    });
                }

                this.name = '';
            }
        },
        hasNewEl(newName) {
            return this.users.find((item) => item.username === newName)
        },
        getNewId() {
            const maxId = this.users.reduce((acc, val) => {
                if (val.id > acc) {
                    return val.id
                }
            }, 0);

            return maxId + 1
        },
    },
    mounted() {
        fetch('https://jsonplaceholder.typicode.com/users')
            .then(response => response.json())
            .then(users => {
                this.users = users;
            });
    }
}
</script>

<style lang="scss" scoped>
    .group-flush {
        margin-top: 20px;

        li {
            display: flex;
            justify-content: space-between;

            span {
                span {
                    display: inline-block;

                    &:last-child {
                        margin-left: 5px;
                        color: #ff4c4c;
                        cursor: pointer;
                    }
                }
            }
        }
    }

    .wrap-add {
        margin-top: 20px;

        button {
            margin-top: 10px;
        }
    }
</style>