<template>
    <form class="form form--login" :submit.prevent="login1">
        <h2 class="form__title">Dashboard login</h2>

        <div class="info info--error" v-if="infoError">Login failed. Please try again.</div>

        <div :class="{'is-waiting': loader}">
            <div class="form-block">
                <input v-model.trim="email" class="field" name="email" type="email" placeholder="E-mail address"
                       required>
            </div>

            <div class="form-block">
                <input v-model.trim="password" class="field" name="password" type="password" placeholder="Password"
                       required>
            </div>

            <div class="form-block form__actions">
                <router-link to="/password-reset">Lost your password?</router-link>
                <button class="button button--green form__submit">Login</button>
            </div>
        </div>
    </form>
</template>




