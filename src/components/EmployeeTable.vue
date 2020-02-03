<template>
    <div id="employee-table">
        <p v-if="employees.length < 1" class="empty-table">
            No Employees
        </p>
        <table v-else>
            <thead>
                <tr>
                    <th>Employee name</th>
                    <th>Employee email</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employees in employees" :key="employees.id">
                    <td v-if="editing === employees.id">
                        <input type="text"  v-model="employees.name" />
                    </td>
                    <td v-else>{{ employees.name }}</td>
                    <td v-if="editing === employees.id">
                        <input type="text"  v-model="employees.email" />
                    </td>
                    <td v-else>{{ employees.email }}</td>
                    <td v-if="editing === employees.id">
                        <button @click="editEmployee(employees)">save</button>
                        <button class="muted-button" @click="cancelEdit(employees)">cancel</button>
                    </td>
                    <td v-else>
                        <button @click="editMode(employees.id)">Edit</button>
                        <button @click="$emit('delete:employee', employees.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        name: 'employee-table',
        props:{
            employees:Array,
        },
        data(){
            return {
                editing: null
            }
        },
        methods: {
            editMode(id){
                this.editing = id
            },
            cancelEdit(employee){
                Object.assign(employee, this.cachedEmployee)
                this.editing = null;
            },
            editEmployee(employee){
                if(employee.name === '' || employee.email === '') return
                this.$emit('edit:employee', employee.id, employee)
                this.editing = null
            },
        }
    }
</script>

<style scoped>
    button{
        margin: 0 0.5rem 0 0;
    }

</style>