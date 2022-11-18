<template>
  <form @submit.prevent="submitInputs" class="social-media-display">
    <div class="social-media-inputs">
      <div
        v-for="(social, key) in socialMediaAccounts"
        :key="key"
        :class="{ 'is-displayed': social.isInputOpen }"
      >
        <div class="input-field" v-if="social.isInputOpen">
          <label>
            <p>{{ formatName(social.name) }}</p>
            <IconsClose @clicked="closeInput(social.name)" />
          </label>
          <input
            type="url"
            v-model="social.value"
            :placeholder="social.placeholder"
            :pattern="social.pattern"
            @invalid="
              $event.target.setCustomValidity(
                `Please enter a valid ${formatName(social.name)} URL`
              )
            "
            @input="$event.target.setCustomValidity('')"
          />
        </div>
      </div>
    </div>
    <div class="social-media-buttons">
      <div v-for="(social, key) in socialMediaAccounts" :key="key">
        <button v-if="!social.isInputOpen" @click="social.isInputOpen = true">
          <img :src="require(`~/static/icons/${social.name}.svg`)" />
          <p>{{ formatName(social.name) }}</p>
        </button>
      </div>
    </div>
    <button
      class="save-button"
      :disabled="!Object.keys(user.socialAccounts).length"
    >
      <p>Save</p>
      <img src="~/static/icons/disk.svg" />
    </button>
  </form>
</template>

<script lang="ts">
import { defineComponent } from "@nuxtjs/composition-api";
export default defineComponent({
  data() {
    return {
      socialMediaAccounts: [
        {
          name: "facebook",
          value: null,
          isInputOpen: false,
          pattern: "^https?://(?:www.)?facebook.com/.+",
          placeholder: "https://www.facebook.com/kangaroodev",
        },
        {
          name: "youtube",
          value: null,
          isInputOpen: false,
          pattern: "^https?://(?:www.)?youtube.com/.+",
          placeholder: "https://youtube.com/kangaroodev",
        },
        {
          name: "twitter",
          value: null,
          isInputOpen: false,
          pattern: "^https?://(?:www.)?twitter.com/.+",
          placeholder: "https://twitter.com/kangaroodev",
        },
        {
          name: "foursquare",
          value: null,
          isInputOpen: false,
          pattern: "^https?://(?:www.)?foursquare.com/user/.+",
          placeholder: "https://foursquare.com/user/1395553878",
        },
        {
          name: "instagram",
          value: null,
          isInputOpen: false,
          pattern: "^https?://(?:www.)?instagram.com/.+",
          placeholder: "https://www.instagram.com/kangaroodev",
        },
        {
          name: "google",
          value: null,
          isInputOpen: false,
          pattern:
            "/\/(u\/\d+\/)?((wm\/[^\/]+)|(b\/\d{21})\/)?(\d{21}|\+[\w_\p{L}-]+)(\/[a-z]+)?\/?/i",
          placeholder: "https://plus.google.com/+kangaroodev/",
        },
        {
          name: "yellow-pages",
          value: null,
          isInputOpen: false,
          placeholder: "https://www.yellow-pages.com/profile/kangaroodev",
          pattern: "^https?://(?:www.)?yellow-pages.com/profile/.+",
        },
        {
          name: "yelp",
          value: null,
          isInputOpen: false,
          placeholder: "https://www.yelp.com/profile/kangaroodev",
          pattern: "^https?://(?:www.)?yelp.com/profile/.+",
        },
        {
          name: "trip-advisor",
          value: null,
          isInputOpen: false,
          placeholder: "https://www.tripadvisor.com/Profile/kangaroodev",
          pattern: "^https?://(?:www.)?tripadvisor.com/Profile/.+",
        },
      ] as Array<{
        name: string;
        value: string | null;
        isInputOpen: boolean;
        pattern: string;
        placeholder: string;
      }>,
    };
  },
  computed: {
    user() {
      const socialAccountsArray: string[][] = [];
      this.socialMediaAccounts.forEach(
        (account) =>
          account.value &&
          socialAccountsArray.push([account.name, account.value])
      );

      const socialAccounts = Object.fromEntries(socialAccountsArray);
      return { socialAccounts };
    },
  },
  methods: {
    capitalize(str: string) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    formatName(str: string) {
      return str
        .replace(/[_-]/g, " ")
        .split(" ")
        .map(this.capitalize)
        .join(" ");
    },
    closeInput(str: string) {
      const socialItem = this.socialMediaAccounts.find(
        (item) => item.name === str
      );
      if (!socialItem) {
        return;
      }
      socialItem.isInputOpen = false;
      socialItem.value = null;
    },
    resetAllValues() {
      this.socialMediaAccounts.forEach((account) => {
        account.value = null;
        account.isInputOpen = false;
      });
    },
    async submitInputs() {
      const axios = require("axios").default;

      await axios
        .post("https://loyalty.kangaroorewards.com/user", this.user)
        .then((response: any) => {
          this.resetAllValues();
          console.log(response);
          alert("Thank you for submitting your infos!");
        })
        .catch((error: any) => {
          this.resetAllValues();
          console.log(error);
          alert("Thank you for submitting your infos!");
        });
    },
  },
});
</script>

<style lang="scss" scoped>
.social-media-display {
  .social-media-buttons {
    display: flex;
    align-items: center;
    width: 100%;
    flex-wrap: wrap;
    margin: 2rem 0;
    button {
      width: fit-content;
      padding: 0.5rem 2rem;
      display: flex;
      border-radius: 1.5rem;
      align-items: center;
      justify-content: center;
      border: 5px dashed $light-blue;
      margin-right: 1rem;
      margin-bottom: 2rem;
      img {
        width: 2rem;
        margin-right: 1rem;
      }
      p {
        color: $main-color;
        font-size: 1.4rem;
        margin-bottom: 0 !important;
      }
    }
    @media screen and (max-width: 800px) {
      justify-content: space-between;
      button {
        border: 3px dashed $light-blue;
        padding: 0.5rem 1rem;
        img {
          width: 1.5rem;
        }
        p {
          font-size: 1rem;
        }
      }
    }
  }
  .social-media-inputs {
    display: flex;
    flex-wrap: wrap;
    margin: 2rem 0;
    > div {
      display: none;
      &.is-displayed {
        display: flex;
        flex-direction: column;
      }
      width: 50%;
      .input-field {
        width: 85%;
        label {
          display: flex;
          justify-content: space-between;
          align-items: center;
          p {
            color: $light-grey;
            font-weight: 500;
            margin-bottom: 0 !important;
            font-size: 1.3rem;
          }
        }
        input {
          background-color: $field-background;
          border-radius: 1.3rem;
          padding: 1rem;
          width: 100%;
          color: $main-color;
          &:focus {
            outline: none;
          }
          ::placeholder{
            color: $lighter-grey;
          }
        }
      }
    }
    @media screen and (max-width: 800px) {
      > div {
        width: 100%;
        margin-bottom: 1rem;
      }
    }
  }
  .save-button {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0.5rem 1rem;
    background-color: $main-color;
    border-radius: 1.8rem;
    float: right;
    &:disabled {
      cursor: not-allowed;
      opacity: 0.3;
    }
    p {
      text-transform: uppercase;
      color: white;
      font-size: 1.3rem;
      font-weight: 500;
      margin-bottom: 0 !important;
      @media screen and (max-width: 800px) {
        font-size: 1rem;
      }
    }
    img {
      margin: 0 0.5rem;
    }
  }
}
</style>
