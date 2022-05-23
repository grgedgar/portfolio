<template>
  <div>
    <div class="header_container">
      <ul class="title_menu">
        <a v-for="(item, index) in items" :key="index" @click="menuItemSelected(item.name)" :title="item.name" :href="item.link" target="_blank" ><li v-bind:class="{ active: chosenItem === item.name }">{{item.name}}</li></a>
      </ul>

      <div class="navbar">
        <div class="nav-container">
          <input class="checkbox" type="checkbox" v-model="checked" name="" id=""/>
          <div class="hamburger-lines">
            <span class="line line1"></span>
            <span class="line line2"></span>
            <span class="line line3"></span>
          </div>
          <div class="menu-items">
            <ul>
              <a v-for="(item, index) in items" :key="index" @click="menuItemSelected(item.name)" :title="item.name" :href="item.link" target="_blank"><li>{{item.name}}</li></a>
            </ul>
          </div>
        </div>
      </div>

      <div class="header_title_container">
        <h2><img class="header_title_img" src="./assets/images/portfolio_title.png" alt="Title image">
        Portfolio - Edgar Grygorian</h2>
      </div>
      <div class="header_adr_container" id="header_adr_container" @click="showAddr('header')" title="Sеnd mеssаge">
        <span v-if="!showAdr.header"><i class='mail icon'></i>Sеnd mеssаge</span>
        <div class="header_my_adr_container" id="header_my_adr_container"></div>
        <button class="clipboard_save_button" v-if="showAdr.header" @click="saveToClipboard('header')" title="Copy to clipboard"><i class="clipboard icon" v-bind:class="{ green_cyan: adrCopied.header === true }"></i></button>
        <div class="adr_copied_msg" v-if="adrCopied.header">Copied to clipboard</div>
      </div>
    </div>

    <div class="overlay" for="checkbox" v-bind:class="{ overlay_active: checked === true }" @click="checked = false"></div>

    <h2 class="body_works_title_h2" id="works_title">Works</h2>
    <div class="body_p_1_hbmk_container">
      <h3 class="body_p_1_hbmk_container_title_h3">Kharkiv Medical College</h3>
      <div class="body_p_1_hbmk_container_image_container">
        <div class="hbmk_themes_switcher">
          <ul>
            <li class="hbmk_light_red_li" v-bind:class="{ switcher_li_active: hbmkSwitcher === 1 }" @click="hbmkSwitcher = 1"></li>
            <li class="hbmk_light_white_smk_li" v-bind:class="{ switcher_li_active: hbmkSwitcher === 2 }" @click="hbmkSwitcher = 2"></li>
          </ul>
        </div>
        <a href="https://grgedgar.github.io/hbmk.org.ua/" target="_blank"><img v-if="hbmkSwitcher === 1" src="https://grgedgar.github.io/portfolio/static/img/hbmk/hbmk_last.1995685.png" alt="Kharkiv Medical College" title="visit Kharkiv Medical College website demo"></a>
        <a href="https://grgedgar.github.io/hbmk.org.ua/" target="_blank"><img v-if="hbmkSwitcher === 2" src="https://grgedgar.github.io/portfolio/static/img/hbmk/hbmk_prev.5fc2257.png" alt="Kharkiv Medical College" title="visit Kharkiv Medical College website demo"></a>
      </div>
      <br>
      <p id="hbmk_p_1"> <i class='pencil icon'></i>The website of Kharkiv Medical College was created on 2010.</p>
      <p id="hbmk_p_2"> <i class='code icon'></i>Almost 500 pages were made from 2010 to 2016</p>
      <p id="hbmk_p_3"> <i class='comments outline icon'></i>Contained a phpBB + mySQL forum</p>
      <a href="https://grgedgar.github.io/hbmk.org.ua/" target="_blank"><button class="visit_hbmk_website_button" title="Visit website demo">Visit website demo</button></a>
    </div>

    <div class="body_p_2_tdkh_container">
      <h3 class="body_p_2_tdkh_container_title_h3">TODES Kharkiv</h3>
      <a href="https://grgedgar.github.io/todes-kh.com/" target="_blank"><img src="https://grgedgar.github.io/portfolio/static/img/tdkh/tdkh_main.31fd270.png" alt="TODES Kharkiv" title="visit TODES Kharkiv website demo" v-bind:class="{ tdkh_main_appear: tdkhAppear.main === true }"></a>
      <br><br>
      <p v-bind:class="{ tdkh_p_appear: tdkhAppear.p1 === true }"><i class='edit outline icon'></i>Self-designed in a form of a T-shirt</p>
      <p v-bind:class="{ tdkh_p_appear: tdkhAppear.p2 === true }"><i class='instagram icon'></i>Instagram API integration</p>
      <p v-bind:class="{ tdkh_p_appear: tdkhAppear.p3 === true }"><i class='desktop icon'></i> <i class='tablet icon'></i><i class='mobile icon'></i>Cross-platform multimedia content support</p>
      <a href="https://grgedgar.github.io/todes-kh.com/" target="_blank"><button class="visit_tdkh_website_button" title="Visit website demo">Visit website demo</button></a>
    </div>

    <div class="body_p_3_others_container">
      <h3 class="body_p_3_others_container_title_h3">Other, but not less important projects</h3>
        <div class="body_p_3_others_container_flexbox_list">
          <a v-for="(project, index) in projects" :key="index" :href="project.link" :title="`Visit ${project.title} project`" :id="`project_${index + 1}`" target="_blank">
            <div>
              <i v-if="project.icon" :class="`${project.icon} icon`"></i>
              <h3 class="project_title">{{project.title}}</h3>
              <p class="project_description">{{project.description}}</p>
            </div>
          </a>
        </div>
    </div>

    <div class="body_p_4_contacts_container">
      <h3 class="body_p_4_contacts_container_title_h3" id="contacts_title">Contacts</h3>
      <div class="body_p_4_contacts_container_show_adr_container" id="adr_container" title="Sеnd mеssаge" @click="showAddr('body')">
        <button class="body_p_4_contacts_adr_ref" v-if="!showAdr.body" title="Sеnd mеssаge"><i class='mail icon'></i>Sеnd mеssаge</button>
        <div class="body_my_adr_container" id="body_my_adr_container"></div>
        <button class="body_clipboard_save_button" v-if="showAdr.body" @click="saveToClipboard('body')" title="Copy to clipboard">
          <i class="clipboard icon" v-bind:class="{ aquamarine_blue: adrCopied.body === true }"></i>
        </button>
        <div class="body_adr_copied_msg" v-if="adrCopied.body">Copied to clipboard</div>
      </div>
    </div>
    <div class="container"></div>
  </div>
</template>

<script>
export default {
  name: 'app',
  mounted() {
    window.addEventListener('scroll', this.scrollActions);
  },
  methods: {
    showAddr(location) {
      const a = `<a href="mailto:${this.unamePart}r@${this.uwebsiteFirstPart}il${this.uwebsiteThirdPart}">${this.unamePart}r@${this.uwebsiteFirstPart}il${this.uwebsiteThirdPart}</>`;
      if (location === 'header') {
        this.showAdr.header = true;
        document.getElementById('header_my_adr_container').innerHTML = a;
      }
      if (location === 'body') {
        this.showAdr.body = true;
        document.getElementById('body_my_adr_container').innerHTML = a;
      }
    },
    menuItemSelected(item) {
      this.chosenItem = item;
      if (this.chosenItem === 'Works') {
        document.getElementById('works_title').scrollIntoView({ block: 'center', behavior: 'smooth' });
      }
      if (this.chosenItem === 'CV') {
        this.openCV();
      }
      if (this.chosenItem === 'Contacts') {
        document.getElementById('contacts_title').scrollIntoView({ block: 'center', behavior: 'smooth' });
      }
    },
    saveToClipboard(location) {
      navigator.clipboard.writeText(`${this.unamePart}r@${this.uwebsiteFirstPart}il${this.uwebsiteThirdPart}`).then(() => {
        if (location === 'header') {
          this.adrCopied.header = true;
        }
        if (location === 'body') {
          this.adrCopied.body = true;
        }
      }, () => {});
    },
    openCV() {
      const downloadAnchorNode = document.createElement('a');
      downloadAnchorNode.setAttribute('href', `ht${this.uadrPartOne}ogl${this.uadrPartTwo}le/d/10d-RJ20IZ-aaOyLH9m5ZkI6GLxnpDm${this.uadrPartThree}ng`);
      downloadAnchorNode.setAttribute('target', '_blank');
      document.body.appendChild(downloadAnchorNode);
      downloadAnchorNode.click();
      downloadAnchorNode.remove();
    },
    scrollActions() {
      if (window.scrollY < 2800) {
        this.chosenItem = 'Works';
      }
      if (window.scrollY > 550 && window.scrollY < 1050) {
        document.getElementById('hbmk_p_1').style.opacity = 1;
      } else {
        document.getElementById('hbmk_p_1').style.opacity = 0;
      }
      if (window.scrollY > 650 && window.scrollY < 1150) {
        document.getElementById('hbmk_p_2').style.opacity = 1;
      } else {
        document.getElementById('hbmk_p_2').style.opacity = 0;
      }
      if (window.scrollY > 750 && window.scrollY < 1250) {
        document.getElementById('hbmk_p_3').style.opacity = 1;
      } else {
        document.getElementById('hbmk_p_3').style.opacity = 0;
      }
      if (window.scrollY > 1100 && window.scrollY < 1700) {
        this.tdkhAppear.main = true;
      } else {
        this.tdkhAppear.main = false;
      }
      if (window.scrollY > 1500 && window.scrollY < 2600) {
        this.tdkhAppear.p1 = true;
      } else {
        this.tdkhAppear.p1 = false;
      }
      if (window.scrollY > 1600 && window.scrollY < 2700) {
        this.tdkhAppear.p2 = true;
      } else {
        this.tdkhAppear.p2 = false;
      }
      if (window.scrollY > 1670 && window.scrollY < 2800) {
        this.tdkhAppear.p3 = true;
      } else {
        this.tdkhAppear.p3 = false;
      }
      if (window.innerWidth > 1600) {
        if (window.scrollY > 2050 && window.scrollY < 3000) {
          this.projects.forEach((element, index) => {
            setTimeout(() => {
              document.getElementById(`project_${index + 1}`).classList.add('others_a_appear');
            }, index * 800);
          });
        } else {
          this.projects.forEach((element, index) => {
            document.getElementById(`project_${index + 1}`).classList.remove('others_a_appear');
          });
        }
      }
      if (window.innerWidth <= 1600) {
        if (window.scrollY > 1700 && window.scrollY < 2800) {
          this.projects.forEach((element, index) => {
            setTimeout(() => {
              document.getElementById(`project_${index + 1}`).classList.add('others_a_appear');
            }, index * 800);
          });
        } else {
          this.projects.forEach((element, index) => {
            document.getElementById(`project_${index + 1}`).classList.remove('others_a_appear');
          });
        }
      }
      if (window.scrollY > 2800) {
        this.chosenItem = 'Contacts';
      }
    },
  },
  data() {
    return {
      windowWidth: 0,
      checked: false,
      items: [
        {
          name: 'Works',
        },
        {
          name: 'Github',
          link: 'https://github.com/grgedgar/',
        },
        {
          name: 'CV',
        },
        {
          name: 'Contacts',
        }],
      chosenItem: 'Works',
      unamePart: 'grgedga',
      uwebsiteFirstPart: 'gma',
      uwebsiteThirdPart: '.com',
      showAdr: {
        header: false,
        body: false,
      },
      adrCopied: {
        header: false,
        body: false,
      },
      uadrPartOne: 'tps://drive.go',
      uadrPartTwo: 'e.com/fi',
      uadrPartThree: 'Au/view?usp=shari',
      hbmkSwitcher: 1,
      tdkhAppear: {
        main: false,
        p1: false,
        p2: false,
        p3: false,
      },
      othersAppear: [
        false,
        false,
        false,
      ],
      projects: [
        {
          icon: 'cloud',
          title: 'PublicAPIs',
          description: 'Vue project for extracting PublicAPIs database',
          link: 'https://grgedgar.github.io/vuejs_public_apis/',
        },
        {
          icon: 'translate',
          title: 'Translator',
          description: 'Vue project with implemented Google Cloud Translate API',
          link: 'https://grgedgar.github.io/vuejs_translator/',
        },
        {
          icon: 'list ul',
          title: 'To-Do List',
          description: 'Vue test project with advanced task settings',
          link: 'https://grgedgar.github.io/vuejs_todo/',
        }],
    };
  },
};
</script>

<style>
  @import "./assets/styles/styles.css";
</style>
