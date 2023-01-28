<template>
    <form @submit.prevent="onFormSubmit">
        <label>Email:</label>
        <input type="email" required v-model="email">

        <label>Password:</label>
        <input type="password" required v-model="password">
        <div v-if="passwordError" class="error">{{ passwordError }}</div>

        <label>Role:</label>
        <select v-model="role">
            <option value="none">None (Choose a Role)</option>
            <option value="developer">Real Dev (C/C++/Rust/Haskell/Assembly)</option>
            <option value="soydeveloper">SoyDev (C#/Java/Javascript/HTML/CSS/Python/...)</option>
        </select>

        <label>Skills:</label>
        <input type="text" v-model="tempSkill" @keyup="skillInputHandler">
        <div v-for="skill in skills" :key="skill" class="skill" @click="deleteSkill(skill)">
            {{ skill }}
        </div>

        <div class="terms">
            <input type="checkbox" v-model="terms" required>
            <label>Accept terms and conditions</label>
        </div>

        <div class="submit">
            <button>Create an Account</button>
        </div>
    </form>

    <p>E-mail: {{ email }}</p>
    <p>Password: {{ password }}</p>
    <p>Role: {{ role }}</p>
    <p>Skills: {{ skills }}</p>
    <p>Terms and Conditions accepted? {{ terms }}</p>
</template>

<script>
export default {
    data() {
        return {
            email: "",
            password: "",
            role: "none",
            terms: false,
            tempSkill: "",
            skills : [],
            passwordError: ""
        }
    },

    methods: {
        skillInputHandler(e) {
            this.tempSkill = this.tempSkill.replaceAll(",", "");
            if (e.key === "," && this.tempSkill.length > 0) {
                if (!this.skills.includes(this.tempSkill)) {
                    this.skills.push(this.tempSkill);                    
                }

                this.tempSkill = "";
            }
        },

        deleteSkill(skill) {
            this.skills = this.skills.filter((item) => { return item !== skill; })
        },

        onFormSubmit() {
            this.passwordError = this.password.length >= 5 ? "" : "The password is too short (min: 5 characters)";
        }
    }
}
</script>

<style>
    form {
        max-width: 420px;
        margin: 30px auto;
        background: white;
        text-align: left;
        padding: 40px;
        border-radius: 10px;
    }
    label {
        color: #aaa;
        display: inline-block;
        margin: 25px 0 15px;
        font-size: 0.6em;
        text-transform: uppercase;
        letter-spacing: 1px;
        font-weight: bold;
    }
    input, select {
        display: block;
        padding: 10px 6px;
        width: 100%;
        box-sizing: border-box;
        border: none;
        border-bottom: 1px solid #ddd;
        color: #555;
    }
    input[type="checkbox"] {
        display: inline-block;
        width: 16px;
        margin: 0 10px 0 0;
        position: relative;
        top: 2px;
    }
    .skill {
        display: inline-block;
        margin: 20px 10px 0 0;
        padding: 6px 12px;
        background: #eee;
        border-radius: 20px;
        font-size: 12px;
        letter-spacing: 1px;
        font-weight: bold;
        color: #777;
        cursor: pointer;
    }
    .submit button {
        background: #0b6dff;
        border: 0;
        padding: 10px 20px;
        margin-top: 20px;
        color: white;
        border-radius: 20px;
        cursor: pointer;
    }
    .submit {
        text-align: center;
    }
    .error {
        color: #ff0062;
        margin-top: 10px;
        font-size: 0.8em;
        font-weight: bold;
    }
</style>
