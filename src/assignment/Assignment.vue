<script setup>
import { ref } from "vue";

const name = ref("");
const studentNumber = ref("");
const gender = ref("");
const address = ref("");

const genderIdentity = ["Male", "Female", "Unknown"];
</script>

<template>
  <div
    class="container px-5 d-flex vh-100 justify-content-center align-items-center"
  >
    <!-- FORM -->
    <div
      class="card w-50 m-5 p-4 pb-5 shadow-sm border-light-subtle bg-primary-subtle"
    >
      <form action="" class="px-2">
        <h4 class="mb-4 text-center text-dark-emphasis fw-bold">
          Student form
        </h4>

        <!-- NAME -->
        <div class="mb-3">
          <label for="Name" class="form-label">Name</label>
          <input
            v-model="name"
            type="text"
            class="form-control"
            id="name"
            placeholder="Condoriano"
          />
        </div>
        <div class="mb-3 d-flex gap-3">
          <!-- STUDENT NUMBER -->
          <div class="w-50">
            <label for="studentNumber" class="form-label">Student number</label>
            <input
              v-model="studentNumber"
              type="text"
              class="form-control"
              id="studentNumber"
              placeholder="20-10020"
            />
          </div>

          <!-- GENDER -->
          <div
            class="w-50 d-flex justify-content-start align-items-start flex-column"
          >
            <label for="gender" class="form-label">Gender</label>
            <select
              v-model="gender"
              class="form-select"
              aria-label="Select gender"
              id="gender"
            >
              <option
                v-for="gender in genderIdentity"
                class="form-control"
                :key="gender"
              >
                {{ gender }}
              </option>
            </select>
          </div>
        </div>

        <!-- ADDRESS -->
        <div>
          <label for="address" class="form-label">Address</label>
          <input
            v-model="address"
            type="text"
            class="form-control"
            id="address"
            placeholder="Obrero Bulan Sorsogon"
          />
        </div>
      </form>
    </div>

    <!-- USER PROFILE -->
    <div
      class="user-profile card w-50 p-4 border-info-subtle d-flex flex-row justify-content-start align-items-center shadow-sm"
    >
      <!-- USER PROFILE -->
      <img v-if="gender == 'Male'" src="/img/male.png" alt="" srcset="" />
      <img
        v-else-if="gender == 'Female'"
        src="/img/female.png"
        alt=""
        srcset=""
      />
      <img v-else src="/img/default-gender.png" alt="" srcset="" />

      <!-- DETAILS -->
      <div class="info mx-4 p-0 d-flex flex-column">
        <!-- NAME -->
        <h4 v-if="name" class="mb-1">{{ name }}</h4>
        <h4 v-else class="mb-1">Name</h4>

        <!-- STUDENT NUMBER -->
        <p v-if="studentNumber" class="text-secondary fw-light">
          {{ studentNumber }}
        </p>
        <p v-else class="text-secondary fw-light">Student number</p>

        <!-- GENDER -->
        <p
          v-if="gender"
          :style="{
            backgroundColor: gender == 'Male' ? 'lightblue' : 'lightgreen',
          }"
          :class="{ 'bg-danger opacity-75 text-light': gender == 'Unknown' }"
          class="px-3 py-1 mb-2 d-inline-block border rounded-pill"
        >
          {{ gender }}
        </p>
        <p
          v-else
          class="text-secondary mb-2 px-3 py-1 rounded-pill border border-secondary-subtle"
        >
          Gender
        </p>

        <!-- ADDRESS -->
        <p
          v-if="address"
          class="text-black px-3 py-1 rounded-pill border border-primary-subtle m-0 text-center"
        >
          {{ address }}
        </p>
        <p
          v-else
          class="text-secondary px-3 py-1 rounded-pill border border-secondary-subtle m-0 text-center"
        >
          Address
        </p>
      </div>
    </div>
  </div>
</template>

<style setup>
.user-profile img {
  width: 10rem;
}
p {
  width: max-content !important;
}
</style>