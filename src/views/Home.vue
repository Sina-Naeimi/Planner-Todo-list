<template>
  <div class="home">
    <v-text-field
      v-model="newTaskTitle"
      outlined
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-3"
      label="Add Task"
      append-icon="mdi-plus"
      hide-details
      clearable
    ></v-text-field>
    <v-list flat class="pt-0">
      <div v-for="item in tasks" :key="item.id">
        <v-list-item :class="{ 'blue lighten-5': item.done }">
          <template v-slot:default>
            <v-list-item-action @click="doneTask(item.id)">
              <v-checkbox :input-value="item.done"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': item.done }"
                >{{ item.title }}</v-list-item-title
              >
            </v-list-item-content>
            <v-list-item-action class="ma-1">
              <v-chip
                color="primary"
                outlined
                :small="isSmall"
                :x-small="isXSmall"
                >{{ item.date }}</v-chip
              >
            </v-list-item-action>
            <v-list-item-action class="ma-1">
              <v-btn
                icon
                @click.stop="dateDialog(item.id)"
                :small="isSmall"
                :x-small="isXSmall"
              >
                <v-icon color="#FB8C00">mdi-calendar</v-icon>
              </v-btn>
            </v-list-item-action>
            <v-list-item-action class="ma-1">
              <v-btn
                icon
                @click.stop="dialogSwitch(item.id)"
                :small="isSmall"
                :x-small="isXSmall"
              >
                <v-icon color="green lighten-1">mdi-pencil</v-icon>
              </v-btn>
            </v-list-item-action>
            <v-list-item-action class="ma-1">
              <v-btn
                icon
                @click.stop="deleteTask(item.id)"
                :small="isSmall"
                :x-small="isXSmall"
                :disabled="loadingDialog"
                :loading="loadingDialog"
              >
                <v-icon color="red lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>

        <v-divider></v-divider>
      </div>
      <v-card v-if="tasks.length == 0" class="text-center mt-16" elevation="0">
        <v-icon x-large color="grey">mdi-check</v-icon>
        <h1 style="color: gray">No Tasks</h1>
      </v-card>
    </v-list>
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title> Edit Task </v-card-title>
        <v-card-text>
          <v-text-field
            v-model="editedTaskTitle[0].title"
            outlined
            @click:append="editTask"
            @keyup.enter="editTask"
            class="pa-3"
            label="Add Task"
            append-icon="mdi-check"
            hide-details
            clearable
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" text @click="dialog = false"> Close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dateDialogSwitch" width="400px">
      <v-date-picker v-model="editedTaskDate[0].date">
        <v-spacer></v-spacer>
        <v-btn text color="primary" @click="dateDialogSwitch = false">
          Cancel
        </v-btn>
        <v-btn text color="primary" @click="editDate()"> OK </v-btn>
      </v-date-picker>
    </v-dialog>
    <v-dialog v-model="loadingDialog" hide-overlay persistent width="300">
      <v-card color="primary" dark>
        <v-card-text>
          Please stand by
          <v-progress-linear
            indeterminate
            color="white"
            class="mb-0"
          ></v-progress-linear>
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-snackbar v-model="addSnackbar" color="green " top>
      Your new task is now added :)

      <template v-slot:action="{ attrs }">
        <v-btn color="yellow" text v-bind="attrs" @click="addSnackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-snackbar v-model="editSnackbar" color="orange" top>
      Your task is now updated :)

      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="editSnackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-snackbar v-model="deleteSnackbar" color="red" top>
      Your task is now deleted :)

      <template v-slot:action="{ attrs }">
        <v-btn
          color="black"
          text
          v-bind="attrs"
          @click="deleteSnackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
/*
import { initializeApp } from "firebase/app";
import {
  getFirestore,
  collection,
  getDocs,
  addDoc,
  deleteDoc,
  doc,
} from "firebase/firestore";

const firebaseConfig = {
  apiKey: "AIzaSyBIYIHzW-XLCKt3dDioSVQeHiSK6ZkYXSA",
  authDomain: "todolist-30ee0.firebaseapp.com",
  databaseURL: "https://todolist-30ee0-default-rtdb.firebaseio.com",
  projectId: "todolist-30ee0",
  storageBucket: "todolist-30ee0.appspot.com",
  messagingSenderId: "367795235751",
  appId: "1:367795235751:web:616466e24a3df270992482",
  measurementId: "G-96D7EVS5PP",
};
// Initialize Firebase app
initializeApp(firebaseConfig);
//initialize services
const db = getFirestore();
//collection ref
const colRef = collection(db, "tasks");
*/
export default {
  name: "Home",
  data() {
    return {
      isLarge: this.$vuetify.breakpoint.lgAndUp,
      isSmall: this.$vuetify.breakpoint.mdOnly,
      isXSmall: this.$vuetify.breakpoint.smAndDown,
      addSnackbar: false,
      editSnackbar: false,
      deleteSnackbar: false,
      dialog: false,
      loadingDialog: false,
      dateDialogSwitch: false,
      newTaskTitle: "",
      editedTaskTitle: [{ id: null, title: null }],
      editedTaskDate: [{ id: null, date: null }],
      tasks: [
        /*  {
        id: 1,
        title: 'wake up',
        done: false,
      },
       {
        id: 2,
        title: 'stand up',
        done: false,
      },*/
      ],
      picker: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10),
    };
  },
  components: {},
  methods: {
   /* getTasks() {
      getDocs(colRef)
        .then((snapshot) => {
          snapshot.docs.forEach((doc) => {
            this.tasks.push({ ...doc.data(), id: doc.id });
            console.log(doc.data())
          });
        })
        .catch((err) => {
          console.log(err.message);
        });
    },*/
    addTask() {
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false,
        date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
          .toISOString()
          .substr(0, 10),
      };
      //addDoc(colRef, newTask);
      this.tasks.push(newTask)
     // this.getTasks();
      this.newTaskTitle = "";
      this.addSnackbar = true;
    },
    editTask() {
      let editedTask = this.tasks.filter(
        (task) => task.id === this.editedTaskTitle[0].id
      )[0];
      editedTask.title = this.editedTaskTitle[0].title;
      this.dialog = false;
      this.editSnackbar = true;
    },
    editDate() {
      let editedDate = this.tasks.filter(
        (task) => task.id === this.editedTaskDate[0].id
      )[0];
      editedDate.date = this.editedTaskDate[0].date;
      this.dateDialogSwitch = false;
      this.editSnackbar = true;
    },
    doneTask(id) {
      let selectedTask = this.tasks.filter((task) => task.id === id)[0];
      selectedTask.done = !selectedTask.done;
    },
    deleteTask(id) {
     // const docRef = doc(db, "tasks", id);
     // deleteDoc(docRef);
     // this.getTasks();
      this.tasks = this.tasks.filter((task) => task.id !== id);
      this.deleteSnackbar = true;
    },
    dialogSwitch(id) {
      let editedTask = this.tasks.filter((task) => task.id === id)[0];
      this.editedTaskTitle[0].title = editedTask.title;
      this.editedTaskTitle[0].id = editedTask.id;
      this.dialog = !this.dialog;
    },
    dateDialog(id) {
      let taskDate = this.tasks.filter((task) => task.id === id)[0];
      this.editedTaskDate[0].date = taskDate.date;
      this.editedTaskDate[0].id = taskDate.id;
      this.dateDialogSwitch = !this.dateDialogSwitch;
    },
  },
 /* created() {
    this.getTasks();
  },
  watch: {
    loadingDialog(val) {
      if (!val) return;

      setTimeout(() => (this.loadingDialog = false), 4000);
    },
  },*/
};
</script>
