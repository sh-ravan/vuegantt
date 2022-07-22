<template>
    <gantt class="left-container" :tasks="tasks" @task-updated="logTaskUpdate" @link-updated="logLinkUpdate" @task-selected="selectTask"></gantt>
</template>

<script>
import Gantt from './components/Gantt.vue'

export default {
  name: 'app',
  components: {Gantt},
  data () {
    return {
      tasks: {
        "data":[
            {"id":"10", "text":"Project #1", "start_date":"01-04-2025", "duration":3, "order":10,"progress":0.4, "open": true},
            {"id":"1", "text":"Task #1",    "start_date":"02-04-2025", "duration":2,  "order":10,"progress":0.6, "parent":"10"},
            {"id":"2", "text":"Task #2",    "start_date":"01-04-2025", "duration":2,  "order":20,"progress":0.6, "parent":"10"},
            {"id":"20", "text":"Project #2", "start_date":"01-04-2025", "duration":3, "order":10,"progress":0.4, "type": "project", "open": true},
            {"id":"3", "text":"Task #3",    "start_date":"02-04-2025", "duration":2,  "order":10,"progress":0.6, "parent":"20"},
            {"id":"4", "text":"Task #4",    "start_date":"01-04-2025", "duration":2,  "order":20,"progress":0.6, "parent":"20"}
        ],
        "links":[
            { "id":1, "source":1, "target":2, "type":"0"},
            { "id":2, "source":2, "target":3, "type":"0"},
            { "id":3, "source":3, "target":4, "type":"0"},
            { "id":4, "source":2, "target":5, "type":"0"}
        ]
    },
    selectedTask: null,
      messages: []
    }
  },
  filters: {
    toPercent (val) {
      if(!val) return '0'
      return Math.round((+val) * 100)
    },
    niceDate (obj){
      return `${obj.getFullYear()} / ${obj.getMonth()} / ${obj.getDate()}`
    }
  },
  methods: {
    selectTask (task) {
      this.selectedTask = task
    },
  
    addMessage (message) {
      this.messages.unshift(message)
      if(this.messages.length > 40) {
        this.messages.pop()
      }
    },

    logTaskUpdate (id, mode, task) {
      let text = (task && task.text ? ` (${task.text})`: '')
      let message = `Task ${mode}: ${id} ${text}`
      this.addMessage(message)
    },

    logLinkUpdate (id, mode, link) {
      let message = `Link ${mode}: ${id}`
      if(link){
        message += ` ( source: ${link.source}, target: ${link.target} )`
      }
      this.addMessage(message)
    }
  }
}
</script>

<style>
  html, body {
    height: 100%;
    margin: 0;
    padding: 0;
  }

  .container {
    height: 100%;
    width: 100%;
  }

  .left-container {
    overflow: hidden;
    position: relative;
    height: 100%;
  }

  .right-container {
    border-right: 1px solid #cecece;
    float: right;
    height: 100%;
    width: 340px;
    box-shadow: 0 0 5px 2px #aaa;
    position: relative;
    z-index:2;
  }

  .gantt-messages {
    list-style-type: none;
    height: 50%;
    margin: 0;
    overflow-x: hidden;
    overflow-y: auto;
    padding-left: 5px;
  }

  .gantt-messages > .gantt-message {
    background-color: #f4f4f4;
    box-shadow:inset 5px 0 #d69000;
    font-family: Geneva, Arial, Helvetica, sans-serif;
    font-size: 14px;
    margin: 5px 0;
    padding: 8px 0 8px 10px;
  }

  .gantt-selected-info {
  border-bottom: 1px solid #cecece;
    box-sizing: border-box;
    font-family: Geneva, Arial, Helvetica, sans-serif;
    height: 50%;
    line-height: 28px;
    padding: 10px;
  }

  .gantt-selected-info h2 {
    border-bottom: 1px solid #cecece;
  }
  
  .select-task-prompt h2{
    color: #d9d9d9;
  }
</style>