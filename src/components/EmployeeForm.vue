<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit()">
            <label>Employee Name</label>
            <input v-model="employee.name" type="text" :class="{'has-error': submitting && invalidName}"  @focus="clearStatus()" @keypress="clearStatus()" ref="first" >
            <label>Employee Email</label>
            <input v-model="employee.email" type="text" :class="{'has-error': submitting && invalidEmail}"  @focus="clearStatus()" @keypress="clearStatus()">
            <p v-if="error && submitting" class="error-message">❗Please fill out all required fields </p>
            <p v-if="success" class="success-message">✅ Employee successfully added </p>
            <button>Add Employee</button>
        </form>
    </div>
</template>

<script>
export default {
    name:'EmployeeForm',
    // This is for rendering the first empty state of the form
    data(){
        return{
            // Or I say this the data object and it can have data of anything
            employee: {
                name: '',
                email: '',
            },
            submitting: false,
            success:false,
            error:false
        }
    },
    computed:{
        invalidName(){
            return this.employee.name === ''
        },

        invalidEmail(){
            return this.employee.email === ''
        },
    },

    methods:{
        handleSubmit(){
            this.submitting = true
            this.clearStatus()

            // The computed property might be stored so we will we use it
            if (this.invalidName || this.invalidEmail) {
                this.error = true
                return
            }
            this.$emit('add:employee',this.employee)
            this.employee={
                name: '',
                email: '',
            }
            this.$refs.first.focus()
            this.error = false
            this.success = true
            this.submitting = false

        },
        clearStatus(){
                this.success = false
                this.error = false
            }
    }
}
</script>

<style>
form{
    margin-bottom: 2rem;
}

[class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }

</style>