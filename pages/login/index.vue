<template>
  <main class="login">
    <LazyCustomHeader>
      <v-container class="center-container">
        <v-row>
          <v-col cols="12" md="8" lg="6" class="mx-auto mt-5">
            <v-row>
              <v-col cols="12">
                <v-card flat color="transparent">
                  <h1 class="text-center login__title mb-12">
                    {{ $t('login_head') }}
                  </h1>
                  <form-wrapper :validator="$v.form">
                    <v-form @submit.prevent="handleLogin">
                      <v-row>
                        <v-col cols="12" class="my-0 py-0">
                          <form-group name="email" attribute="email">
                            <template slot-scope="{ attrs, listeners }">
                              <v-text-field
                                v-on="listeners"
                                v-bind="attrs"
                                background-color="#fff"
                                filled
                                v-model="form.email"
                                type="email"
                              ></v-text-field>
                            </template>
                          </form-group>
                        </v-col>
                        <v-col cols="12" class="my-0 py-0">
                          <form-group name="password" attribute="password">
                            <template slot-scope="{ attrs, listeners }">
                              <v-text-field
                                v-bind="attrs"
                                v-on="listeners"
                                filled
                                background-color="#fff"
                                v-model="form.password"
                                :type="!showPassword ? 'password' : 'text'"
                                :label="$t('password')"
                                :append-icon="passwordIcon"
                                @click:append="showPassword = !showPassword"
                              ></v-text-field>
                            </template>
                          </form-group>
                        </v-col>
                        <v-col cols="12" class="my-0 py-0 mb-3">
                          <v-row class="align-center">
                            <v-col cols="5" class="my-0 py-0 transparency">
                              <v-checkbox
                                dense
                                class="login__remember-me"
                                :label="$t('remember_me')"
                              ></v-checkbox>
                            </v-col>
                            <v-col
                              cols="7"
                              class="my-0 py-0"
                              :class="
                                $i18n.locale === 'ar'
                                  ? 'text-sm-left'
                                  : 'text-sm-right'
                              "
                            >
                              <nuxt-link
                                style="color: #fff"
                                :to="localePath('/forget-password')"
                              >
                                {{ $t('forget_password') }}
                              </nuxt-link>
                            </v-col>
                          </v-row>
                        </v-col>
                        <v-col cols="12" class="my-0 py-0">
                          <v-btn
                            :disabled="$v.form.$invalid"
                            height="56px"
                            type="submit"
                            block
                            large
                            class="primary text-capitalize"
                            >{{ $t('login') }}
                          </v-btn>
                        </v-col>
                        <v-col cols="12" class="my-0 py-0">
                          <v-btn
                            class="mt-5 text-capitalize"
                            color="#fff"
                            style="border-color: #fff"
                            outlined
                            height="56px"
                            block
                            large
                            :to="toRegisterPage"
                            >{{ $t('register') }}</v-btn
                          >
                        </v-col>
                      </v-row>
                    </v-form>
                  </form-wrapper>
                </v-card>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </LazyCustomHeader>
  </main>
</template>

<script>
import { required, email } from 'vuelidate/lib/validators'
export default {
  name: 'Login',
  layout: 'auth',
  middleware: ['auth'],
  data() {
    return {
      showPassword: false,
      form: {
        email: '',
        password: '',
        type: 'USER',
      },
    }
  },
  methods: {
    handleLogin() {
      this.$auth
        .loginWith('local', { data: this.form })
        .then(() => {
          this.$router.push(this.localePath('/'))
        })
        .catch((err) => {
          console.log(err)
        })
    },
    googleLogin() {
      this.$auth.loginWith('google')
    },
  },
  computed: {
    passwordIcon() {
      if (!this.showPassword) {
        return 'mdi-eye-off'
      } else {
        return 'mdi-eye'
      }
    },
    toRegisterPage() {
      if (this.$route.query.type === 'COMPANY') {
        return this.localePath(`/register?type=${this.$route.query.type}`)
      } else {
        return this.localePath('/register')
      }
    },
  },
  watch: {
    $route: {
      handler({ query }) {
        if (query.type) {
          const availbleTypes = ['USER', 'COMPANY']
          if (availbleTypes.includes(query.type)) {
            this.form.type = query.type
          } else {
            this.form.type = 'USER'
          }
        }
      },
      immediate: true,
    },
  },
  validations: {
    form: {
      email: {
        required,
        email,
      },
      password: {
        required,
      },
    },
  },
}
</script>

