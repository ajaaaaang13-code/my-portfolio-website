<script setup>
    import {ref, onMounted, onBeforeUnmount} from "vue";
    import axios from "axios";

    import {Notyf} from "notyf";
    import 'notyf/notyf.min.css';

    const notyf = new Notyf();


    const WEB3FORMS_ACCESS_KEY = "b6bff544-d968-4555-981f-8c273e0ba750";
    const SITE_KEY = "6LdovRouAAAAAOefqW2Xg5gNnXUPWTauTcKgmXiM";
    // Initial value for the email:
    const subject = "New message from Portfolio Contact Form";

    const name = ref("");
    const email = ref("");
    const message = ref("");

    const isLoading = ref(false);

    // Function for sending our email:
    const submitForm = async() => {
        if (!recaptchaToken.value) {
                notyf.error('Please verify that you are not a robot.');
                return;
            }

        isLoading.value = true

        try{
            const response = await axios.post("https://api.web3forms.com/submit",{
                access_key: WEB3FORMS_ACCESS_KEY,
                subject: subject,
                name: name.value,
                email: email.value,
                message: message.value
            },{
                headers: {
                    'Content-Type' : "application/json",
                    Accept: "application/json"
                }
            }
        )

                const result = response.data;

                if(result.success){
                    notyf.success("Message Sent!");

                    name.value = "";
                    email.value = "";
                    message.value = "";
                }

            }catch(error){
                notyf.error("Failed to send email!");
            } finally{
                isLoading.value = false;
            }
        }


        const recaptchaContainer = ref(null);
        const recaptchaWidgetId = ref(null);
        const recaptchaToken = ref('');

        // Callback called by reCAPTCHA when successful
        function onRecaptchaSuccess(token) {
          recaptchaToken.value = token;
        }

        // Callback when expired
        function onRecaptchaExpired() {
          recaptchaToken.value = '';
        }

        // Function to render the reCAPTCHA widget
        function renderRecaptcha() {
          if (!window.grecaptcha) {
            console.error('reCAPTCHA not loaded');
            return;
          }

          recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            size: 'normal', // or 'compact'
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired,
          });
        }

        // Function to reset reCAPTCHA 
        function resetRecaptcha() {
          if (recaptchaWidgetId.value !== null) {
            window.grecaptcha.reset(recaptchaWidgetId.value);
            recaptchaToken.value = '';
          }
        }



        onMounted(() => {
          // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
          // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
          // Callback functions handle success and expiration events. 
          // reCAPTCHA is reset upon form submission to clear the token.
          const interval = setInterval(() => {
            if (window.grecaptcha && window.grecaptcha.render) {
              renderRecaptcha();
              clearInterval(interval);
            }
          }, 100);

          onBeforeUnmount(() => {
            clearInterval(interval);
          });
        });

</script>

<template>
  <!-- Contact -->
  <section id="contact" class="content-section content-width contact-section" aria-labelledby="contact-title">
    <div class="contact-details">
      <img src="/images/pic.jpeg" alt="Portrait of Angelica Janine Reyes">
      <p class="eyebrow">Stay connected</p>
      <h2>Let's work together!</h2>
      <p><a href="tel:+639271345656">Call me: +63 927 257 7475</a></p>
      <p><a href="mailto:ajaaaaang13@gmail.com">ajaaaaang13@gmail.com</a></p>
    </div>

    <div class="contact-form">
      <h2 id="contact-title">Contact Me</h2>
      <p>Have a project in mind? Let's work together! I'd love to hear from you.</p>

      <form @submit.prevent ="submitForm">
        <div class="form-grid">
            <div class="form-field">
              <label for="name">Name</label>
              <input v-model="name" id="name" name="name" type="text" autocomplete="name" placeholder="Your name">
            </div>
            <div class="form-field">
              <label for="email">Email</label>
              <input v-model="email" id="email" name="email" type="email" autocomplete="email" placeholder="Your email">
            </div>
            <div class="form-field">
              <label for="number">Number</label>
              <input v-model="number" id="number" name="number" type="tel" autocomplete="tel" placeholder="Mobile number">
            </div>
            <div class="form-field">
              <label for="company">Company</label>
              <input v-model="company" id="company" name="company" type="text" autocomplete="organization" placeholder="Company">
            </div>
            <div class="form-field form-field-wide">
              <label for="message">Message</label>
              <textarea v-model="message" id="message" name="message" rows="5" placeholder="How can I help?"></textarea>
            </div>
        </div>
         <button type="submit" class="btn btn-primary pl-5 pr-5" :disabled="isLoading"> {{isLoading ? "Sending..." : "Submit"}}</button>
         
         <!-- recaptcha checkbox -->
            <div class="d-flex justify-content-end mt-2">
            <div ref="recaptchaContainer"></div>
          </div>
      </form>
    </div>
  </section>
</template>
