<script context="module">
  export const prerender = true;
</script>

<script lang="ts">
  import type { Form } from "$lib/types/form.type";
  import OpenGraph from "$lib/components/open-graph.svelte";
  import Textarea from "$lib/components/ui-library/textarea";
  import Input from "$lib/components/ui-library/input";
  import Select from "$lib/components/ui-library/select";
  import Button from "$lib/components/ui-library/button";
  import Card from "$lib/components/ui-library/card";

  import { countryList } from "$lib/contents/license-key";
  import type { Email } from "../functions/submit-form";
  import Header from "$lib/components/header.svelte";
  import { cloudPlatforms, noOfEngineers } from "$lib/contents/contact";
  import Checkbox from "$lib/components/ui-library/checkbox";
  import { tick } from "svelte";
  import { scrollToElement } from "../lib/utils/helpers";
  import SubmissionSuccess from "$lib/components/submission-success.svelte";

  const formData: Form = {
    firstName: {
      el: null,
      valid: false,
      value: "",
    },
    consent: {
      el: null,
      valid: false,
      checked: false,
    },
    lastName: {
      el: null,
      valid: false,
      value: "",
    },
    email: {
      el: null,
      valid: false,
      value: "",
    },
    company: {
      el: null,
      valid: false,
      value: "",
    },
    country: {
      el: null,
      valid: false,
      value: "",
    },
    noOfDevelopers: {
      el: null,
      valid: false,
      value: "",
    },
    cloudInfrastructure: {
      el: null,
      valid: false,
      value: "",
    },
    message: {
      el: null,
      valid: true,
      value: "",
    },
  };

  let isFormDirty: boolean = false;
  let form: HTMLElement;
  let isEmailSent: boolean = false;
  let sectionStart: HTMLElement;

  $: isFormValid = Object.values(formData).every((field) => field.valid);

  const handleSubmit = async () => {
    isFormDirty = true;
    if (!isFormValid) {
      await tick();
      scrollToElement(form, ".error");
      return;
    }

    const email: Email = {
      toType: "sales",
      replyTo: {
        email: formData.email.value,
        name: `${formData.firstName.value} ${formData.lastName.value}`,
      },
      subject:
        "Requesting a professional Self-Hosted license" +
        "  (from " +
        formData.email.value +
        ")",
      message: `
        ${formData.company.value}
        ${formData.firstName.value} ${formData.lastName.value}
        
        developers: ${formData.noOfDevelopers.value}
        Cloud Infrastructure: ${formData.cloudInfrastructure.value}
        Message:
        ${formData.message.value}
      `,
    };

    try {
      const response = await fetch("/.netlify/functions/submit-form", {
        method: "POST",
        body: JSON.stringify(email),
      });
      if (response.ok) {
        isEmailSent = true;
        setTimeout(() => {
          sectionStart.scrollIntoView();
        });
      } else {
        console.error(response.statusText);
      }
    } catch (error) {
      console.error(error);
    }
  };
</script>

<style lang="postcss">
  .title:not(:first-child) {
    margin-top: var(--medium);
  }

  .link {
    @apply underline;
  }
</style>

<OpenGraph
  data={{
    description: "Request a free community license",
    title: "Community License",
    norobots: true,
  }}
/>

<Header tight={true}>
  <div slot="content">
    <h1 class="h2">Request free community license</h1>
    <p>Fill out the form and receive the free self-hosted community license.</p>
  </div>
</Header>

<Card
  size="small"
  class="shadow-normal p-xx-small sm:py-small sm:px-x-small md:p-medium mb-32 sm:mx-8"
>
  <div bind:this={sectionStart}>
    {#if isEmailSent}
      <SubmissionSuccess
        title="Check your email"
        text="We've just sent you your license key via email. Enjoy!"
      />
    {:else}
      <form bind:this={form} on:submit|preventDefault={handleSubmit} novalidate>
        <h2 class="h4 title">Customer Information</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-y-4 gap-x-small">
          <div>
            <Input
              label="First Name*"
              hasError={isFormDirty && !formData.firstName.valid}
              name="first-name"
              id="first-name"
              type="text"
              bind:value={formData.firstName.value}
              bind:element={formData.firstName.el}
              on:change={() => {
                formData.firstName.valid =
                  formData.firstName.value &&
                  formData.firstName.el.checkValidity();
              }}
              autocomplete="given-name"
            />
          </div>
          <div>
            <Input
              hasError={isFormDirty && !formData.lastName.valid}
              label="Last Name*"
              name="last-name"
              id="last-name"
              type="text"
              bind:value={formData.lastName.value}
              bind:element={formData.lastName.el}
              on:change={() => {
                formData.lastName.valid =
                  formData.lastName.value &&
                  formData.lastName.el.checkValidity();
              }}
              autocomplete="family-name"
            />
          </div>
          <div>
            <Input
              label="Work Email*"
              hasError={isFormDirty && !formData.email.valid}
              type="email"
              name="e-mail"
              id="email"
              bind:value={formData.email.value}
              bind:element={formData.email.el}
              on:change={() => {
                formData.email.valid =
                  formData.email.value && formData.email.el.checkValidity();
              }}
              autocomplete="email"
            />
          </div>
          <div>
            <Input
              hasError={isFormDirty && !formData.company.valid}
              label="Company*"
              name="company"
              id="company"
              bind:value={formData.company.value}
              bind:element={formData.company.el}
              on:change={() => {
                formData.company.valid =
                  formData.company.value && formData.company.el.checkValidity();
              }}
              type="text"
              autocomplete="organization"
            />
          </div>
          <div>
            <Select
              hasError={isFormDirty && !formData.country.valid}
              label="Country*"
              name="country"
              bind:value={formData.country.value}
              bind:element={formData.country.el}
              on:change={() => {
                formData.country.valid =
                  formData.country.value && formData.country.el.checkValidity();
              }}
              class="option"
              autocomplete="country"
              options={countryList}
              placeholder="Select..."
            />
          </div>
          <div>
            <Select
              placeholder="Select..."
              options={noOfEngineers}
              label="Total number of engineers*"
              hasError={isFormDirty && !formData.noOfDevelopers.valid}
              name="noOfDevelopers"
              bind:value={formData.noOfDevelopers.value}
              bind:element={formData.noOfDevelopers.el}
              on:change={() => {
                formData.noOfDevelopers.valid =
                  formData.noOfDevelopers.value &&
                  formData.noOfDevelopers.el.checkValidity();
              }}
            />
          </div>
          <div>
            <Select
              label="Cloud infrastructure*"
              hasError={isFormDirty && !formData.cloudInfrastructure.valid}
              name="cloudInfrastructure"
              bind:value={formData.cloudInfrastructure.value}
              on:change={(e) => {
                formData.cloudInfrastructure.valid =
                  formData.cloudInfrastructure.value &&
                  // @ts-ignore
                  e.target.validity.valid;
              }}
              options={cloudPlatforms}
              placeholder="Which cloud infrastructure do you use?"
              class="max-w-md"
            />
          </div>
        </div>
        <div class="mt-4">
          <Textarea
            label="Optionally, tell us more about your interest in Gitpod. What challenges
          are you looking to solve? How can we help?"
            cols="30"
            rows="4"
            bind:value={formData.message.value}
            bind:element={formData.message.el}
            name="message"
            id="message"
          />
          <div class="my-4">
            <Checkbox
              hasError={isFormDirty && !formData.country.valid}
              label="I consent to having this website store my submitted information so that the sales team can respond to my inquiry."
              bind:checked={formData.consent.checked}
              bind:element={formData.consent.el}
              on:change={() => {
                formData.consent.valid =
                  formData.consent.checked &&
                  formData.consent.el.validity.valid;
              }}
            />
          </div>
          <div class="mt-4">
            <p class="text-sm my-4">
              By submitting this form I acknowledge that I have read and
              understood <a class="link" href="/privacy"
                >Gitpod’s Privacy Policy.</a
              >
            </p>
            <Button
              variant="primary"
              size="large"
              type="submit"
              disabled={isFormDirty && !isFormValid}>Receive license</Button
            >
          </div>
          {#if isFormDirty && !isFormValid}
            <legend class="text-xs text-error block mt-1 mb-2">
              Please fill out all required fields above
            </legend>
          {/if}
        </div>
      </form>
    {/if}
  </div>
</Card>
