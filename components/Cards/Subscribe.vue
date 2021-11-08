<template>
  <v-card class="employee-card" flat>
    <nuxt-link
      class="d-block"
      :to="
        localePath({
          path: `/profile/info?employee=${employee.user.id}`,
          query: { ...$route.query },
        })
      "
    >
      <!-- <img
        :src="employee.avatar || require('@/assets/images/avatar.png')"
        alt="photo"
      /> -->
      <section class="img-wrap ratio-100">
        <div class="img-content radius-10">
          <img
            :src="employee.avatar || require('@/assets/images/avatar.png')"
            alt=""
          />
        </div>
      </section>
    </nuxt-link>
    <v-card-text>
      <v-row>
        <v-col cols="12">
          <h2 class="employee-card__title mb-1">
            {{ employee.user ? employee.user.first_name : '' }}
            {{ employee.user ? employee.user.last_name : '' }}
          </h2>
          <div class="employee-card__time">
            <v-icon color="#A1A1A1" small>mdi-clock</v-icon>
            <span>{{ employee.user ? employee.user.created_at : '' }}</span>
          </div>
              <div class="employee_completed_profile_ratio">
            <p>
              {{ $t('completed_profile_ratio') }}
              {{  employee.user.completedProfileRatio }} %</p>
          </div>
        </v-col>
        <v-col cols="12" md="6">
          <v-btn
            :href="employee.cv ? employee.cv.file : ''"
            target="_blank"
            large
            width="100%"
            class="radius-10 text-capitalize"
            color="primary"
            :disabled="employee.cv == ''"
          >
            <strong class="mt-1"
              ><small>{{ $t('download_cv') }}</small></strong
            >
          </v-btn>
        </v-col>
        <v-col cols="12" md="6">
          <v-btn
            @click="$emit('on-dialog-open', employee.id)"
            outlined
            large
            width="100%"
            class="radius-10 text-capitalize"
            color="primary"
          >
            <strong class="mt-1">{{ $t('send_message') }}</strong>
          </v-btn>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: 'Employee',
  props: {
    employee: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      dialog: false,
    }
  },
}
</script>

