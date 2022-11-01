<template>
  <div class="about">
    <v-card elevation="0">
      <v-container>
        <v-row>
          <v-col md="6" cols="12">
            <v-card elevation="0">
              <v-card-title class="justify-center">
                <h1>About Me</h1>
              </v-card-title>
              <v-card-text class="justify-center">
                Familiar with the process of software maintenance and
                development, familiar with rewriting and improving software
                performance,I have a team spirit and eager to learn new content
                and update my knowledge
              </v-card-text>
              <v-divider></v-divider>
              <h1>Experience</h1>
              <v-list-item
                three-line
                v-for="(experience, index) in experiences"
                :key="index"
              >
                <v-list-item-content>
                  <v-list-item-title>{{ experience.title }} </v-list-item-title>
                  <v-list-item-title
                    >{{ experience.subtitle }}
                  </v-list-item-title>
                  <v-list-item-subtitle>
                    {{ experience.text }}
                  </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-divider></v-divider>
              <h1>Skills</h1>
              <v-card-actions
                v-for="(skill, index) in skills"
                :key="index"
                class="pa-4"
              >
                {{ skill.title }}
                <v-spacer></v-spacer>
                <span class="grey--text text--lighten-2 text-caption mr-2">
                  ({{ skill.rate }})
                </span>
                <v-rating
                  readonly
                  :value="skill.rate"
                  background-color="white"
                  color="yellow accent-4"
                  dense
                  half-increments
                  size="18"
                ></v-rating>
              </v-card-actions>
              <v-divider></v-divider>
              <h1>Educations</h1>
              <v-list-item
                v-for="(education, index) in educations"
                :key="index"
              >
                <v-list-item-content>
                  <v-list-item-title
                    >{{ education.university }}
                  </v-list-item-title>
                  <v-list-item-title
                    >{{ education.subject }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-divider></v-divider>
              <h1>Language</h1>
              <h3>English</h3>
              <v-card-actions
                v-for="(skill, index) in englishLang"
                :key="index"
                class="pa-4"
              >
                {{ skill.skill }}
                <v-spacer></v-spacer>
                <span class="grey--text text--lighten-2 text-caption mr-2">
                  ({{ skill.rate }})
                </span>
                <v-rating
                  readonly
                  :value="skill.rate"
                  background-color="white"
                  color="yellow accent-4"
                  dense
                  half-increments
                  size="18"
                ></v-rating>
              </v-card-actions>
            </v-card>
          </v-col>
          <v-col md="6" cols="12" style="text-align: center">
            <v-card elevation="0">
              <v-avatar
                color="primary"
                :size="$vuetify.breakpoint.mdAndUp ? '400' : '300'"
                ><v-img src="me.jpg"></v-img
              ></v-avatar>
              <v-card-title class="justify-center"
                ><h1>Sina Naeimi</h1></v-card-title
              >
              <v-card-subtitle class="mt-1"
                >Full-Stack Web Developer</v-card-subtitle
              >
              <v-card-actions class="justify-end">
                <v-list flat>
                  <v-list-item
                    v-for="(information, index) in informations"
                    :key="index"
                  >
                    <v-list-item-content>
                      <a :href="information.link">
                        <v-list-item-title
                          v-text="information.info"
                        ></v-list-item-title>
                      </a>
                    </v-list-item-content>
                    <v-list-item-icon>
                      <v-icon v-text="information.icon"></v-icon>
                    </v-list-item-icon>
                  </v-list-item>
                </v-list>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-card>
  </div>
</template>
<script>
export default {
  data: () => ({
    experiences: [
      {
        title: "Naeimbreak | Tehran",
        subtitle: "Programming instructor | 10/2021 - Untill now",
        text: "Teaching HTML, CSS, JavaScript, Vue.js, Vuetify, Axios",
      },
      {
        title: "Kamex | Tehran",
        subtitle: "Front-End Developer | 10/2021 - 02/2022",
        text: "Rewrite web pages using HTML, CSS, Vue, Vuetify",
      },
      {
        title: "Rayan pajouh salamt | Tehran",
        subtitle: "Junior Web Developer | 05/2021 - 09/2021",
        text: "Maintenance, develop and debug a LMS, Rewrite the site to improve performance and speed using .Net and Vue.js",
      },
      {
        title: "Ostooreh | Tehran",
        subtitle: "IT Assistant | 08/2020 - 05/2021",
        text: "Developer, Editor and Supporter",
      },
    ],
    skills: [
      { title: "Vue", rate: 4.5 },
      { title: "Vuetify", rate: 4.5 },
      { title: "Axios", rate: 3 },
      { title: "JavaScript", rate: 4.5 },
      { title: "Vuetify", rate: 4.5 },
      { title: "HTML", rate: 4.5 },
      { title: "CSS", rate: 4.5 },
      { title: "C#", rate: 3.5 },
      { title: ".NET", rate: 3.5 },
      { title: "Git", rate: 4.5 },
      { title: "Clean code", rate: 4 },
    ],
    educations: [
      {
        university:
          "Islamic Azad University Central Tehran Branch | Tehran, Tehran Province",
        subject: "Computer engineering",
      },
    ],
    englishLang: [
      { skill: "Reading", rate: 4 },
      { skill: "Writing", rate: 4 },
      { skill: "Speaking", rate: 4 },
      { skill: "Listening", rate: 4 },
    ],
    germanLang: [
      { skill: "Reading", rate: 1 },
      { skill: "Writing", rate: 1 },
      { skill: "Speaking", rate: 1 },
      { skill: "Listening", rate: 1 },
    ],
    informations: [
      { info: "naeimbreak@gmail.com", icon: "mdi-email" },
      { info: "+98 9336753722", icon: "mdi-phone" },
      { info: "Iran,Tehtan,Chitgar", icon: "mdi-map-marker" },
      {
        info: "sina-naeimi",
        icon: "mdi-github",
        link: "https://github.com/sina-naeimi",
      },
      {
        info: "sina-naeimi-423b94191",
        icon: "mdi-linkedin",
        link: "https://www.linkedin.com/in/sina-naeimi-423b94191",
      },
    ],
  }),
};
</script>
<style>
a{
  text-decoration: none;
}
</style>