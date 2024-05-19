<template>
  <div id="app">
    <div class="flex justify-center">
    <h3>Kanban Board</h3>
      <div class="min-h-screen flex overflow-x-scroll py-12 flex-container">
        <div
          v-for="column in columns"
          :key="column.title"
          class="bg-gray-100 rounded-lg px-3 py-3 column-width rounded mr-4 column">
          <span class="drag-column-header">
              <p class="text-gray-700 font-semibold font-sans tracking-wide text-sm" contenteditable="true">{{column.title}}</p>
              <button class="add-task-button" :id="column.title" contenteditable="false"
                  @click="addTask(column.title)"
                  >+</button>
          </span>
          <hr>
          <!-- Draggable component comes from vuedraggable. It provides drag & drop functionality -->
          <draggable :list="column.tasks" :animation="200" ghost-class="ghost-card" group="tasks"
          @change="updateList(column)">
            <!-- Each element from here will be draggable and animated. Note :key is very important here to be unique both for draggable and animations to be smooth & consistent. -->
            <TaskCard
              v-for="(task) in column.tasks"
              :key="task.id"
              :task="task"
              :columns="columns"
              class="mt-3 cursor-move drag-item">
            </TaskCard>
            <!-- </transition-group> -->
          </draggable>
        </div>
        <button class="deleteButton" @click="addSection()" contenteditable="false">+Add Section</button>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import TaskCard from "./components/TaskCard.vue";
export default {
  name: "App",
  components: {
    TaskCard,
    draggable
  },
  data() {
    return {
      columns: [
        {
          title: "ToDo",
          tasks: [
            {
              "id": 1,
              "title": "Add discount code to checkout page",
              "description": "In the checkout page a discount code is to be added depending on the logged in user credentials and usage of the platform by the user",
              "date": "Sep 14",
              "Assigned": "Sayak Bhattacharjee",
              "type": "ToDo"
            },
            {
              "id": 2,
              "title": "Implement user authentication system",
              "description": "Develop a secure user authentication system with features like login, logout, and password recovery.",
              "date": "Sep 20",
              "assigned": "John Doe",
               "type": "ToDo"
            },
            {
              "id": 3,
              "title": "Optimize database queries",
              "description": "Review and optimize database queries to improve application performance.",
              "date": "Sep 25",
              "assigned": "Jane Smith",
               "type": "ToDo"
            },
            {
              "id": 4,
              "title": "Design homepage layout",
              "description": "Create wireframes and design mockups for the homepage layout of the website.",
              "date": "Sep 18",
              "assigned": "Alice Johnson",
               "type": "ToDo"
            },
          ]
        },
        {
          title: "InProgress",
          tasks: [
            {
              "id": 5,
              "title": "Update product images",
              "description": "Replace existing product images with high-resolution images for better visual appeal.",
              "date": "Sep 22",
              "assigned": "Emily Wilson",
               "type": "InProgress"
            },
            {
              "id": 6,
              "title": "Fix broken links",
              "description": "Identify and fix broken links across the website to ensure a seamless user experience.",
              "date": "Sep 28",
              "assigned": "Alex Thompson",
               "type": "InProgress"
            },
            {
              "id": 7,
              "title": "Write user documentation",
              "description": "Create comprehensive user documentation covering all aspects of the application.",
              "date": "Sep 30",
              "assigned": "Sophia Miller",
               "type": "InProgress"
            }
          ]
        },
        {
          title: "Done",
          tasks: [
            {
              "id": 8,
              "title": "Implement payment gateway integration",
              "description": "Integrate a secure payment gateway to allow users to make purchases on the platform.",
              "date": "Oct 5",
              "assigned": "Michael Brown",
              "type": "Done"
            },
            {
              "id": 9,
              "title": "Conduct user testing",
              "description": "Organize and conduct usability testing sessions to gather feedback from users.",
              "date": "Oct 3",
              "assigned": "David Martinez",
              "type": "Done"
            },
            {
              "id": 10,
              "title": "Create marketing campaign",
              "description": "Develop a marketing campaign to promote new product features and attract more users.",
              "date": "Oct 8",
              "assigned": "Jessica Garcia",
              "type": "Done"
            }
          ]
        }
      ]
    };
  },
  methods: {
    addSection: function(){
      this.columns.push(
        {
          title: "Please add the title of the workflow state here...",
          tasks: []
        },
      )
    },
    addTask: function(columnTitle){
      this.columns.forEach(element => {
        if(element.title === columnTitle){
          element.tasks.push(
            {
              "id": element.tasks.length + 1,
              "title": "Please add the task title here...",
              "description": "Please add a short description of the task here...",
              "date": "please mention the due date of this task here...",
              "Assigned": "Please mention the assignee of this task here...",
              "type": columnTitle
            }
          )
        }
      })
    },
    updateList: function(column){
      column.tasks.forEach(element => {
        if(element.type != column.title){
          element.type = column.title;
        }
      })
    }
  }
};
</script>

<style scoped>
  .cardHeader{
    display: flex;
    justify-content: space-between;
  }
  .headerContent {
      display: flex;
      align-items: center;
    }

    .deleteButton {
        margin-left: auto;
        font-size: x-large;
        background: none;
        border: none;
        cursor: pointer;
    }
    .taskDescription {
      margin-left: 24px; /* Adjust this margin according to your needs */
      /* Retain the class styles */
      color: #4a5568; /* text-gray-700 */
      font-weight: 600; /* font-semibold */
      font-family: sans-serif; /* font-sans */
      letter-spacing: 0.025em; /* tracking-wide */
      font-size: 0.875rem; /* text-sm */
  }
  .custom-element {
    padding-left: 0.75rem;
    padding-right: 0.75rem;
    height: 1.5rem;
    border-radius: 9999px;
    font-size: 0.75rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    background-color: #e0f2f1;
    color: #00796b;
  }
  .badgeClass {
    width: 2rem;
    height: 2rem;
    border-radius: 50%;
    margin-right: 0.25rem;
  }
  .flex-container {
    display: flex;
    overflow-x: auto;
  }

  .column {
    flex: 1; 
    min-width: 320px;
    margin-right: 20px;
    background-color: #edf2f7;
    border-radius: 8px;
    padding: 16px;
  }

  .column:last-child {
    margin-right: 0;
  }
  .drag-column-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    //padding: 0px;
    //height: 8vh;
  }
  .drag-column .drag-column-header > div {
    width: 100%;
  }
  .add-task-button {
    font-size: x-large;
    margin-left: auto;
  }
  .drag-item {
    padding: 10px;
    margin: 10px;
    height: auto;
    background: rgba(0, 0, 0, 0.4);
    transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
  }
  .doneClassCard {
    background-color: #8ED1FC;
    color: #fff;
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  .inProgressClassCard {
    background-color: #4dfc9ea6;
    color: #333;
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  .todoClassCard {
    background-color: #FFA0A0;
    color: #fff;
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
</style>


