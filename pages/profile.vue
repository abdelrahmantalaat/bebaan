<template>
  <main class="profile">
    <section class="profile__wrapper">
      <LazyCustomHeader>
        <v-container class="profile__wrapper--container">
          <v-row>
            <v-col cols="12" md="8">
              <div class="profile__wrapper--right">
                <v-row>
                  <v-col cols="12" sm="6">
                    <LazyImageUpload
                      :imgUrl="profile.avatar && profile.avatar"
                      :authType="$auth.user.type"
                    />
                  </v-col>
                  <v-col cols="12" md="6">
                    <div class="profile__wrapper--right__personal-info">
                      <h1
                        class="profile__wrapper--right__personal-info__title"
                        v-if="profile"
                      >
                        {{
                          profile.personal_information
                            ? profile.personal_information.first_name
                            : '' + ' ' + profile.personal_information
                            ? profile.personal_information &&
                              profile.personal_information.last_name
                            : ''
                        }}
                      </h1>
                      <p
                        class="
                          profile__wrapper--right__personal-info__sub-title
                        "
                      >
                        {{ profile ? profile.job_title : '' }}
                      </p>
                    </div>
                    <v-list flat color="transparent">
                      <template v-for="(item, index) in items">
                        <v-list-item v-if="item.title" :key="index">
                          <v-list-item-icon>
                            <v-icon color="#fff">{{ item.icon }}</v-icon>
                          </v-list-item-icon>
                          <v-list-item-content>
                            <v-list-item-title
                              style="font-size: 18px; color: #fff"
                            >
                              {{ item.title }}
                            </v-list-item-title>
                          </v-list-item-content>
                        </v-list-item>
                      </template>
                    </v-list>
                    <v-col cols="12">
                      <v-row>
                        <v-col cols="6">
                          <!-- profile.cv && profile.cv[0].file -->
                          <v-btn
                            :href="profile.cv[1].file"
                            target="_blank"
                            height="54px"
                            color="#fff"
                            outlined
                          >
                            <img
                              width="23px"
                              height="21px"
                              target="_blank"
                              :src="require('@/assets/images/upload.svg')"
                              alt="photo"
                            />
                            <span class="mx-2">{{ $t('cv') }}</span>
                          </v-btn>
                        </v-col>
                        <v-col cols="4" class="mx-auto">
                          <v-btn
                            :href="profile.cv[1].file"
                            width="61px"
                            height="54px"
                            color="#fff"
                            outlined
                          >
                            <img
                              width="23px"
                              height="21px"
                              :src="require('@/assets/images/printing.svg')"
                              alt="photo"
                            />
                          </v-btn>
                        </v-col>
                      </v-row>
                      <v-row>
                        <template v-for="(social, key) in socials">
                          <v-col v-if="social.link" cols="1" :key="key">
                            <v-btn
                              icon
                              link
                              :href="social.link"
                              target="_blank"
                            >
                              <v-icon color="#fff">{{ social.icon }}</v-icon>
                            </v-btn>
                          </v-col>
                        </template>
                      </v-row>
                    </v-col>
                  </v-col>
                </v-row>
              </div>
            </v-col>
            <v-col cols="12" md="4">
              <div class="profile__wrapper--left">
                <div class="profile__wrapper--left__shape-container">
                  <v-progress-circular
                    :rotate="90"
                    :size="150"
                    :width="15"
                    :value="profile.completedProfileRatio"
                    color="primary"
                  >
                    <h2>{{ `${profile.completedProfileRatio}%` }}</h2>
                  </v-progress-circular>
                </div>
                <h4 class="pt-5" style="color: #fff">
                  {{ $t('complete_you_cv') }}
                </h4>
                <p class="pt-5" style="color: #fff">
                  <v-icon color="#fff">mdi-calendar</v-icon>
                  <span>{{ $t('last_updated_cv') }} </span>
                  <span>{{ profile.latest_updated_at }}</span>
                </p>
                <v-row>
                  <v-col cols="12">
                    <v-btn block class="warning">
                      <v-icon>mdi-crown</v-icon>
                      <span class="font-bold" style="font-size: 11px">
                        {{ $t('Upgrade to Premium') }}</span
                      >
                    </v-btn>
                  </v-col>
                  <!-- <v-col cols="12" md="6">
                    <v-btn block class="primary">
                      <v-icon>mdi-heart</v-icon>
                      <span class="font-bold" style="font-size: 11px">{{
                        $t('Asked assistant')
                      }}</span>
                    </v-btn>
                  </v-col> -->
                </v-row>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </LazyCustomHeader>
    </section>

    <nuxt-child :nuxt-child-key="$route.fullPath" />
  </main>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'Profile',
  middleware: ['auth'],

  async fetch({ store, route }) {
    await store.dispatch('profile/setUserProfile', route.query)
  },

  data() {
    return {
      socials: {},
      value: 25,
    }
  },
  computed: {
    ...mapGetters({
      profile: 'profile/getProfile',
    }),
    items() {
      return [
        {
          title: `${
            this.profile.personal_information &&
            this.profile.personal_information.country
              ? this.profile.personal_information.country.name[
                  this.$i18n.locale
                ]
              : ''
          } - ${
            this.profile.personal_information &&
            this.profile.personal_information.city
              ? this.profile.personal_information.city.name[this.$i18n.locale]
              : ''
          }`,
          icon: 'mdi-map-marker',
        },
        {
          title: this.profile.contacts && this.profile.contacts.email,
          icon: 'mdi-email',
        },
        {
          title: this.profile.contacts && this.profile.contacts.phone,
          icon: 'mdi-phone',
        },
      ]
    },
  },
  watch: {
    profile: {
      handler({ socials }) {
        this.socials = {
          facebook: { icon: 'mdi-facebook', link: socials && socials.facebook },
          instagram: { icon: 'mdi-instagram', link: socials && socials.insta },
          twitter: { icon: 'mdi-twitter', link: socials && socials.twitter },
          site: { icon: 'mdi-earth', link: socials && socials.site },
          medium: {
            icon: 'mdi-facebook-gaming',
            link: socials && socials.medium,
          },
        }
      },
      immediate: true,
    },
  },
}
</script>

