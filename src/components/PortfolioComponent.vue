<template>
  <div class="terminal" @click="focusInput">
    <div class="content">
      <p v-for="(line, index) in displayLines" 
         :key="index" 
         v-html="line.replace(/ /g, '&nbsp;')">
      </p>
    </div>
    <div class="portfolio" v-if="showPortfolio">
      <div class="left-content">
        <img src="/mike-alt.png" alt="Image" id="img1" />
        <div class="title">Mike Weatherford</div>
        <div class="subtitle">Software Developer</div>
        <div class="email-git-link">
          <a href="mailto:mikesweatherford@gmail.com" target="_blank">> email-address</a>
        </div>
        <div class="email-git-link">
          <a href="https://github.com/GiorgioMotorola" target="_blank">> git-hub-page</a>
        </div>
        <div class="email-git-link">
          <a href="https://www.linkedin.com/in/michael-weatherford-10a3ab220/" target="_blank">> linkedin-page</a>
        </div>
      </div>

      <div class="right-content">
        <div class="about-me">About Me</div>
        <div class="about-me-desc">I am a software developer with an insatiable curiosity and a strong passion for learning and creating. My journey started in 2022 with the University of Helsinki's Java MOOC, and I've since successfully completed the Software Development and Web Development courses offered by the state-funded coding bootcamp called Code: You. Shortly after, I would work under contract as a software development intern at Virtual Peaker. Currently, I am a Level 2 Service Desk Analyst with Encore Technologies.</div>
        <div class="project-title">Projects</div>
        <div class="links-container">
          <div class="link-item" v-for="(item, index) in links" :key="index">
            <a :href="item.url" target="_blank">
              <img 
                :src="item.currentSrc || item.imgSrc" 
                :alt="item.imgAlt" 
                class="link-image"
                @mouseover="item.currentSrc = item.hoverImgSrc" 
                @mouseleave="item.currentSrc = item.imgSrc"
              />
              <div class="link-text">{{ item.text }}</div>
            </a>
          </div>
        </div>
      </div>
    </div>

    <div class="input-line" v-if="!showPortfolio">
  <span>&gt;&nbsp;</span>
  <input v-model="input" @keyup.enter="processInput" ref="inputField" autofocus />
</div>
  </div>
  <div class="scanline"></div>
</template>


<script>
export default {
  data() {
    return {
      input: '',
      started: false,
      showPortfolio: false,
      lines: [
        "Initializing system...",
        "Loading modules: [██████████]█ 110%?",
        "Authenticating... Access Ulysses S. Granted",
        "Fetching encrypted data... [OK], Prepossessing Cyphers ... [OK], Building Steam With A Grain Of Salt... [OK]",
        "Decoding stream: 01100010 01101001 01101110 01100001 01110010 01111001",
        "Running diagnostic height distribution: Procompsognathus... [PASS]",
        "Compiling source code... Compiling source code... Compiling source code... Compiling source code...",
        "This ain't no party... This ain't no disco... This ain't no foolin' around...",
        "System status: OPERATIONAL",
        "Finalizing startup procedures... Complete.",
        "Hello :)",
        " ",
      ],
      displayLines: ["type 'login user' and press enter"],
      currentLine: '',
      charIndex: 0,
      lineIndex: 0,
      links: [
        { url: 'https://nostatic.mweatherford.rocks/', imgSrc: '/nostatic.PNG', hoverImgSrc: '/nostatic-hover.PNG', imgAlt: 'Image 1', text: 'No Static: An Essential Albums Guide' },
        { url: 'https://weather.mweatherford.rocks/', imgSrc: '/weather.PNG', hoverImgSrc: '/weather-hover.PNG', imgAlt: 'Image 2', text: 'The Weather-ford Cast' },
        { url: 'https://lastyear.mweatherford.rocks/', imgSrc: '/lastfm.PNG', hoverImgSrc: '/lastfm-hover.PNG', imgAlt: 'Image 3', text: 'Last Year' },
        { url: 'http://www.accardi.xyz/', imgSrc: '/joe.PNG', hoverImgSrc: '/joe-hover.PNG', imgAlt: 'Image 4', text: 'PowerShell Terminal Portfolio Website' },
        { url: 'https://ff.mweatherford.rocks/', imgSrc: '/ff.PNG', hoverImgSrc: '/ff-hover.PNG', imgAlt: 'Image 5', text: 'Fighting Frames' },
        { url: 'https://randwiki.mweatherford.rocks/', imgSrc: '/rand.PNG', hoverImgSrc: '/rand-hover.PNG', imgAlt: 'Image 6', text: 'Rand.Wiki' }
      ]

    };
  },
  methods: {
    processInput() {
      if (this.input.trim().toLowerCase() === 'login user') {
        this.started = true;
        this.displayLines = [];
        this.currentLine = '';
        this.charIndex = 0;
        this.lineIndex = 0;
        this.typeLine();
      }
      this.input = '';
    },
    typeLine() {
      if (this.lineIndex >= this.lines.length) {
        this.showPortfolio = true;
        return;
      }

      const fullText = this.lines[this.lineIndex];
      this.currentLine = '';
      this.displayLines.push("");

      const typeChar = () => {
        if (this.charIndex < fullText.length) {
          this.displayLines[this.lineIndex] += fullText[this.charIndex];
          this.charIndex++;
          setTimeout(typeChar, 10);
        } else {
          this.lineIndex++;
          this.charIndex = 0;
          if (this.lineIndex < this.lines.length) {
            setTimeout(this.typeLine, 300);
          } else {
            this.showPortfolio = true;
          }
        }
      };

      typeChar();
    }
  }
};
</script>

<style scoped>
* {
  margin: 0;
  font-family: Inconsolata, monospace;
}

*::-webkit-scrollbar {
  display: none;
}

.terminal {
  background: #3f250d;
  background-image: url("/noise.png");
  color: #fdad44;
  font-family: monospace;
  height: 100vh;
  width: 100vw;
  padding: 15px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  overflow: hidden;
  font-size: 17px;
  top: 0;
  left: 0;
}

.content {
  transition: transform 1s ease-in-out;
}

.input-line {
  display: flex;
}

input {
  background: #3f250d;
  color: #fdad44;
  border: hidden;
  font: 1.3rem Inconsolata, monospace;
  outline: none;
  width: 25%;
  font-size: 17px;
  display: hidden;
  background-image: url("/noise.png");
  caret-color: #fdad44 !important;
}

.portfolio {
  display: flex;
  justify-content: space-between; 
  align-items: flex-start; 
  background: #3f250d;
  color: #fdad44;
  text-align: center;
  background-image: url("/noise.png");
  overflow: auto;
  border-top: hidden;
}

.left-content {
  display: flex;
  flex-direction: column; 
  align-items: flex-start; 
  gap: 0px; 
  margin: 2rem;
}

.project-title, .about-me {
  font-size: 55px;
  color: #4494FD;
}

.about-me-desc {
  font-size: 15px;
  text-align: start;
  padding: 1rem;
  margin-left: 30rem;
  margin-bottom: 2rem;
}

.right-content {
  display: flex;
  flex-direction: column;
  align-items: flex-end; 
  margin: 2rem;
}

#img1 {
  box-shadow: rgba(6, 24, 44, 0.4) 0px 0px 0px 2px, rgba(6, 24, 44, 0.65) 0px 4px 6px -1px;
  max-width: 250px;
  height: auto;
  border-radius: 50%;
}

.title, .subtitle, .email-git-link {
  text-align: start;
}

.title {
  font-size: 28px;
  color: #4494FD;
}

.subtitle {
  font-size: 20px;
  color: #FC9712;
}

.email-git-link {
  font-size: 15px;
}

a {
  text-decoration: none;
  color: #fdad44;
}

.desc {
  font-size: 15px;
}

.links-container {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  text-wrap: wrap;
  margin-bottom: 4rem;
}

.link-item {
  text-align: center;
  margin: 10px;
  max-width: 150px;
  word-wrap: break-word;
}

.link-image {
  width: 150px;
  height: 150px;
  object-fit: cover;
  border-radius: 3px;
}

.link-text {
  margin-top: 10px;
  font-size: 16px;
  color: #fdad44;
  word-wrap: break-word; 
  word-break: break-word;
  text-align: center;
}

@keyframes scanline {
  from {
    top: 0%;
  }
  to {
    top: 94%;
  }
}

.scanline {
  position: absolute;
  width: 100%;
  height: 50px;
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 0 5px rgba(255, 255, 255, 0.2);
  animation: scanline 3s linear infinite;
  mix-blend-mode: overlay;
}

@media screen and (max-width: 1300px) {
  
  .portfolio {
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  .left-content, .right-content {
    align-items: center;
    margin: 1rem 0;
  }

  .about-me-desc {
    margin-left: 0;
    text-align: center;
    padding: 1rem;
  }

  @media screen and (max-width: 1000px) {

  .terminal {
    padding: 10px;
    font-size: 12px;
    word-wrap: break-word;
  }

  .links-container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
    justify-content: center;
  }

  .link-item {
    max-width: none;
    width: 95%;
    text-align: center;
  }
}

@media screen and (max-width: 600px) {
  .terminal {
    padding: 5px;
    font-size: 10.5px;
    word-wrap: break-word;
  }

  .link-text {
    font-size: 10px;
  }

  .input-line {
    font-size: 10px;
  }

  input {
    font-size: 10px;
  }
}

}

</style>
