<template>
  <section class="home">
    <div class="md:py-36 mx-auto flex flex-wrap flex-col md:flex-row items-center">
      <div class="flex flex-col w-full xl:w-3/5 justify-center lg:items-start overflow-y-hidden">
        <div v-html="$md.render(welcomeText)" class="home__welcome markdown" />
  
      </div>
      <div class="flex flex-col w-full xl:w-2/5">
        <img
          alt="Hero"
          class="rounded shadow-xl"
          src="@/static/images/uploads/rute.PNG"
        />
      </div>
      
      <div class="home__welcome markdown">
      <h1> Information</h1>
       <ul> 
          <li> - Port Kode = 1234 + firkant </li> <br>
          <li> - Din tomme trailer må ALDRIG stilles i port 5 (Vareindlevering). </li><br>
          <li> - Vores tomme trailere må stilles i Port 1-7 og den læssede trailer står som regel i port 3. </li><br>
          <li> - Kontaktperson: Lagermand Børge TLF. 20126157. Kan kontaktes vedrørende manglendee følgesedler og hjælp ved læsning.</li><br>
          <li> - Vi kører både stykgods og full loads for denne kunde. Ring hvis du er i tvivl om hvad du skal med din trailer.</li><br>
          <li> - Ved stykgods: Kør til Frejas Terminal Ørstedsvej 11, 8660 Skanderborg og HUSK at melde traileren til på terminalkontoret. </li><br>
          <li> - Trailertype: Denne kunde bruger trailer med og uden lift + Box trailer. </li><br>
        </ul>
        </div>
    </div>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import settings from '@/content/settings/general.json';

@Component({
  // Called to know which transition to apply
  transition() {
    return 'slide-left';
  },
})
export default class Home extends Vue {
  welcomeText = settings.welcomeText;

  get posts(): Post[] {
    return this.$store.state.posts;
  }

  isSignedUp = false;

  form = {
    email: '',
  };

  encode(data): string {
    return Object.keys(data)
      .map((key) => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`)
      .join('&');
  }

  validEmail(email): boolean {
    // eslint-disable-next-line
    const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(email);
  }

  async handleSubmit(): Promise<void> {
    if (!this.validEmail(this.form.email)) {
      this.$refs.emailInput.focus();
      return;
    }

    try {
      await fetch('/', {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: this.encode({ 'form-name': 'signups', ...this.form }),
      });

      this.isSignedUp = true;
    } catch (error) {
      console.error(error);
    }
  }
}
</script>
