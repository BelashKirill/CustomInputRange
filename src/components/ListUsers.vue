<template>
    <input class="form-control" list="datalistOptions" id="exampleDataList" placeholder="search..." v-model="search" @input="searchName(copyArr)">
    <ul class="list-group list-group-flush group-flush">
        <li class="list-group-item" v-for="(name) in jsonData" :key="name.id">
            <span>{{name[0]}}</span>
            <span>
                <a :href="'/users/' + name[1]">Перейти</a>
                <a href="#" @click="deleteItem(name)">Удалить</a>
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
            jsonData: null,
            search: null,
            copyArr: null,
            name: null
        }
    },
    methods: {
        searchName(copyArr) {
            let resultArr = [];
            let pattern = new RegExp(this.search, "i");

            for (let i = 0; i < this.copyArr.length; i++) {
                if (pattern.test(copyArr[i])) {
                    resultArr.push(copyArr[i]);
                }
            }

            this.jsonData = resultArr;
        },
        deleteItem(name) {
            let getIndex = this.jsonData.indexOf(name);
            let getIndexCopy = this.copyArr.indexOf(name);

            this.jsonData.splice(getIndex, 1);
            this.copyArr.splice(getIndexCopy, 1);
        },
        addItem() {
            if (this.name) {
                if (this.jsonData.indexOf(this.name) === -1) {
                    this.jsonData.push(this.name);
                }

                if (this.copyArr.indexOf(this.name) === -1) {
                    this.copyArr.push(this.name);
                }

                this.name = '';
            }
        }
    },
    mounted() {
        fetch('https://jsonplaceholder.typicode.com/users')
            .then(response => response.json())
            .then(users => {
                let arrNames = [];

                for (let user in users) {
                    arrNames.push([users[user].username, users[user].id]);
                }

                this.jsonData = arrNames;
                this.copyArr = arrNames;
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
                a {
                    &:first-child {
                        margin-right: 5px;
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