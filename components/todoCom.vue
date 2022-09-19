<template>
    <div>
        <v-text-field dense outlined solo flat label="Enter task and Hit Enter" v-model="task" @change="storeTask_FNC">
        </v-text-field>
        <v-alert dense :type="alertType" v-if="alertShow">{{alertText}}</v-alert>
        <v-simple-table>
            <thead>
                <tr>
                    <th>Tasks Detail</th>
                    <th></th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, i) in todos" :key="item.id">
                    <td>
                        <div>
                            {{i+1}}. <span class="ml-2"
                                :class="[item.is_done ? 'text-decoration-line-through' : '']">{{item.task}}</span>
                        </div>
                    </td>
                    <td align="end">
                        <v-btn color="primary" v-if="item.is_done" @click="updateStatus(item, i)">Not Done</v-btn>
                        <v-btn color="primary" v-else @click="updateStatus(item, i)">Done</v-btn>
                    </td>
                    <td align="end">
                        <v-btn color="error" @click="deleteTask(item,i)">Remove</v-btn>
                    </td>
                </tr>
            </tbody>
        </v-simple-table>
    </div>
</template>

<script>
export default {
    data: () => ({
        task: '',
        todos: [],
        btnText: "Not Done",
        alertText: null,
        alertType: null,
        alertShow: false,
    }),

    async fetch() {
        this.todos = await fetch('http://localhost:8000/api/todos').then(res => res.json());
    },
    methods: {
        storeTask_FNC() {
            this.$axios.post("http://localhost:8000/api/todos", { task: this.task }).then(res => {
                this.todos.unshift(res.data);
                this.alertText = "Added Successfully";
                this.alertType = "success";
                this.alertShow = true;
                this.task = ''
                setTimeout(() => {
                    this.alertShow = false;
                }, 2000);
                window.location.reload()
            }).catch(error => {
            }).finally(() => {
            });
        },
        updateStatus(val, i) {
            this.$nuxt.$loading.start();
            let api = "http://localhost:8000/api/todos/" + val.id;
            this.$axios.put(api).then(res => {
                this.todos[i].is_done = res.data.is_done
            }).catch(error => {
            }).finally(() => {
                this.$nuxt.$loading.finish();
            });
        },
        deleteTask(val, i) {
            this.$nuxt.$loading.start();
            let api = "http://localhost:8000/api/todos/" + val.id;
            this.$axios.delete(api).then(res => {
                if (res.data == true) {
                    this.todos.splice(i, 1);
                }
            }).catch(error => {
            }).finally(() => {
                this.$nuxt.$loading.finish();
            });
        }
    }
}
</script>

<style>

</style>